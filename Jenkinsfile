library 'shared-library'
pipeline{
    agent any
    environment {
        lifestory = "This is a story about a person named Shamini who loves to code."
    }
    parameters {
        string(name: 'Name', defaultValue: 'Shamini', description: 'Enter the name of the person')

        text(name: 'Details', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'Female', defaultValue: true, description: 'Is the person')

        choice(name: 'Emotion', choices: ['happy', 'sad', 'dont know'], description: 'Pick your current emotion')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages{
        stage("env and params"){
            steps{
                echo "Hello Everyone, ${env.lifestory} "
                echo "Name: ${params.Name}"
                echo "Details: ${params.Details}"
                echo "i am: ${params.Female}"
                echo "My current emotion is: ${params.Emotion}"
                echo "My password is: ${params.PASSWORD}" 
                 echo "My current branch is ${env.BRANCH_NAME}"  
            }
        }
        stage("conditions"){
            when {
                expression { // multiple conditions can be used here like equals, not, and, or, environment variables, notequals, etc.
                    // This stage will run only if the branch is main or master
                    BRANCH_NAME == 'main' || BRANCH_NAME == 'master'
                }
            }
            steps{
                echo "This stage runs only on the main or master branch."
            }
        }
        stage("groovy file invokation"){
            steps{
                script {
                    example() // This function is defined in the shared library
                }
            }
        }
        stage("groovy file in pipeline"){
            steps{
                script {
                    // load a example groovy file and call a function from it
                    def example = load 'example.groovy' 
                    example.callFunction() // Call the function defined in example.groovy
                }
            }
        }
    }
    post {
        //Only run this, when the current pipeline or a specific stage has a success
        success{
            echo "Pipeline completed successfully!"
        }
    }
}
