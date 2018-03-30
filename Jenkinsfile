pipeline {
    agent any

    stages {
        stage('git') {
            cleanWs()
            git 'https://github.com/BenjaminGrimault/jenkins-exercise-5'
        }
        stage("updateConf") {
        // On récupère les credentials que l'on a créé au préalable dans Jenkins (http://localhost:8080/credentials/)
        // Ici c'est prod_user, prod_database et prod_password qui sont les valeurs contenues dans env/integration.properties
        // Pour rappel l'avantage des credentials est de sécuriser et centraliser les données sensibles qui peuvent être contenues dans une application
            withCredentials([
                string(credentialsId: params.user, variable: 'user'),
                string(credentialsId: params.database, variable: 'database'),
                string(credentialsId: params.password, variable: 'password')
            ]) {
                // On lit le contenu du fichier conf/bdd.conf
                // afin de pouvoir remplacer les valeurs null par celle de l'environement d'integration
                // Etant donné que l'on ne peut pas décocher la sandbox de groovy on change de système pour mettre à jour le fichier
                String config = readFile "conf/bdd.conf"

                // La méthode replace renvoie une copie de la chaîne remplacée
                // d'où le config = config.replace()
                // https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#replace-java.lang.CharSequence-java.lang.CharSequence-
                config = config.replace("[user]", user)
                config = config.replace("[database]", database)
                config = config.replace("[password]", password)

                // Maintenant que l'on a fait les modifications du json on les sauvegarde dans le fichier
                writeFile file: "conf/bdd.conf", text: config
            }
        }
        stage("saveConf") {
            // On précise à jenkins d'archiver tous les fichiers qui sont dans conf/
            // Vous pourrez retrouver l'archive sur la page du job pour vérifier que cela à bien fonctionné
            archiveArtifacts 'conf/*'
        }
    }
}
