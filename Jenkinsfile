pipeline {

    agent any

    environment {
        ANDROID_SDK_ROOT="/Users/alangrubb/Library/Android/sdk"
        PATH = "/usr/local/bin:/usr/local/lib/ruby/gems/2.7.0/bin:/usr/local/opt/ruby/bin:$PATH"
        PATH_TO_APP_APK = "/Users/alangrubb/workspace/espresso-browserstack/app/build/outputs/apk/debug/app-debug.apk"
        PATH_TO_ANDROID_TEST_APK = "/Users/alangrubb/workspace/espresso-browserstack/app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk"
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

        stage('BrowserStack CLI to Run Tests') {
            steps {
                sh "browserstack app-automate espresso run --app=${PATH_TO_APP_APK} --testSuite=${PATH_TO_ANDROID_TEST_APK}"
            }
        }

        // stage('Upload App apk to BrowserStack') {
        //     steps {
        //         sh "curl -u \"${BROWSERSTACK_USERNAME}:${BROWSERSTACK_ACCESS_KEY}\" \
        //             -X POST \"https://api-cloud.browserstack.com/app-automate/upload\" \
        //             -F \"file=@${PATH_TO_APP_APK}\""
        //     }
        // }

        // stage('Upload androidTest apk to BrowserStack') {
        //     steps {
        //         sh "curl -u \"alangrubb2:ndfaSQAUFEm7oyk23Uyk\" \
        //             -X POST \"https://api-cloud.browserstack.com/app-automate/espresso/test-suite\" \
        //             -F \"file=@${PATH_TO_ANDROID_TEST_APK}\""
        //     }
        // }

        // stage('Execute tests on BrowserStack') {
        //     steps {
        //         sh 'npm run ios'
        //     }
        // }
    }
}