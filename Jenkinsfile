node {
  stage 'Checkout'
  checkout scm
  stage 'Download Composer'
  sh "wget -q https://getcomposer.org/download/1.1.1/composer.phar -O composer.phar"
  stage 'Install dependencies'
  sh "php -f composer.phar -- install"
  stage 'Run tests'
  sh "./vendor/bin/phpunit -c phpunit.xml.dist"
  stage 'Run metrics'
  sh "if [ ! -d ./reports ]; then mkdir ./reports ; fi"
  sh "./vendor/bin/phploc --log-xml ./reports/phploc.xml ./src"
  sh "./vendor/bin/phpmd ./src xml --reportfile ./reports/pmd.xml --exclude *TestsDataFixtures*,*TestCase.php,*Test.php"
  sh "./vendor/bin/phpcpd --log-pmd ./reports/pmd-cpd.xml ./src"
  sh "./vendor/bin/phpcs --report=checkstyle --extensions=php --encoding=utf-8 --report-file=./reports/checkstyle.xml ./src"
}
