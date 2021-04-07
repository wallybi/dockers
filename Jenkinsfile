/*#Ejemplo de script declarativo en Jenkins
#Andrés Alcaraz Rey
#•
#4 mar
#Para consulta

#jenkinsfile.txt
#Texto
#Comentarios de la clase

#Ejemplo de script declarativo en Jenkins */
pipeline {
            agent any
                stages {
                        stage('Descargar') {
                            steps {
                                echo 'descargando...'
                                sh 'rm -R * || true'
                                sh 'rm -R .* || true'
                                sh 'git clone https://github.com/andresalcarazrey/tiendaweb.git .'
                                echo 'Llamando a compose'
                                sh 'composer require --dev phpunit/phpunit | composer require  phpmailer/phpmailer'
                            }
                        }
                        stage('Test') {
                            steps {
                                echo 'testeando'
                                sh '/var/lib/jenkins/workspace/TiendaWebPipelined/vendor/bin/phpunit --log-junit ./results/phpunit/phpunit.xml -c phpunit.xml'
                            }
                        }
                        stage('Deploy') {
                            steps {
                                echo 'Desplegando'
                                sh 'cp /var/lib/jenkins/workspace/TiendaWebPipelined/*.html /var/www/html/'
                                sh 'cp /var/lib/jenkins/workspace/TiendaWebPipelined/*.php /var/www/html/'
                                sh 'cp /var/lib/jenkins/workspace/TiendaWebPipelined/*.json /var/www/html'
                                sh 'composer -d /var/www/html require phpmailer/phpmailer'
                            }
                        }
                }
            post {
                always {
                    echo 'Pipeline en ejecución'
                }
                success {
                    echo 'Parece que todo ha ido bien'
                }
                failure {
                    echo 'Algo ha fallado'
                    mail to: "wallybi@gmail.com",
                         subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                         body: "Algo ha fallado con ${env.BUILD_URL}"
                }
            }
}
