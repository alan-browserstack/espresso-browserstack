pipeline {

    agent any

    environment {
        ANDROID_SDK_ROOT="/Users/alangrubb/Library/Android/sdk"
        PATH = "/usr/local/bin:/usr/local/lib/ruby/gems/2.7.0/bin:/usr/local/opt/ruby/bin:$PATH"
        PATH_TO_APP_APK = "./app/build/outputs/apk/debug/app-debug.apk"
        PATH_TO_ANDROID_TEST_APK = "./app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk"
    }

    stages {
        stage('Check ENV and get browserstack cli') {
            steps {
                echo "username: ${BROWSERSTACK_USERNAME}"
                echo "key: ${BROWSERSTACK_ACCESS_KEY}"
                sh "browserstack authenticate --username=${BROWSERSTACK_USERNAME} --access-key=${BROWSERSTACK_ACCESS_KEY}"
            }
        }

        stage('Clean') {
            steps {
                sh './gradlew clean'
            }
        }

        stage('Build app apk') {
            steps {
                sh './gradlew assembleDebug'
            }
        }

        stage('Build androidTest apk') {
            steps {
                sh './gradlew assembleAndroidTest'
            }
        }

        stage('Upload test suite to BrowserStack for execution') {
            steps {
                sh "browserstack app-automate espresso run --app=${PATH_TO_APP_APK} --testSuite=${PATH_TO_ANDROID_TEST_APK} --devices=\"Google Pixel 3-10.0\""
                echo 'View your test suite: https://app-automate.browserstack.com/dashboard/v2'
            }
        }
    }
}