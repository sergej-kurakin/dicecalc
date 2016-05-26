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
  parallel (
    phploc: {
      sh "./vendor/bin/phploc --log-xml ./reports/phploc.xml ./src || true"
    },
    phpmd: {
      sh "./vendor/bin/phpmd ./src xml cleancode,codesize,controversial,design,naming,unusedcode --reportfile ./reports/pmd.xml --exclude *TestsDataFixtures*,*TestCase.php,*Test.php || true"
    },
    phpcpd: {
      sh "./vendor/bin/phpcpd --log-pmd ./reports/pmd-cpd.xml ./src || true"
    },
    phpcs: {
      sh "./vendor/bin/phpcs --report=checkstyle --extensions=php --encoding=utf-8 --report-file=./reports/checkstyle.xml ./src || true"
    }
  )
  step([$class: 'PmdPublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: './reports/pmd.xml', unHealthy: ''])
  step([$class: 'CheckStylePublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: './reports/checkstyle.xml', unHealthy: ''])
}
