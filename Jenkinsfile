pipeline{
    agent any
    stages{
        stages('Build'){
            steps{
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}