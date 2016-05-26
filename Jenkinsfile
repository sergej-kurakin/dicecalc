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
}
