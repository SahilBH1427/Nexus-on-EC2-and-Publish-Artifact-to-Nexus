This project demonstrates how to set up a Nexus Repository Manager on an EC2 instance and publish a Java application's JAR artifact to it using Gradle.

🧰 Tech Stack
☁️ AWS EC2 (Amazon Linux 2)

📦 Sonatype Nexus Repository OSS

☕ Java 17

⚙️ Gradle 8+

🐧 Linux (EC2 + CLI)

🐙 Git + GitHub

🚀 Project Overview
Launch EC2

Install Nexus Repository

Configure Nexus and create a Maven hosted repo

Configure build.gradle to publish your app

Publish .jar to Nexus from local system

✅ View artifact on Nexus UI

🖼️ Screenshots
<img width="1807" height="774" alt="Screenshot 2025-07-11 051004" src="https://github.com/user-attachments/assets/ad9384a6-f341-46d5-b911-6bbbd30e578d" />
<img width="1916" height="903" alt="Screenshot 2025-07-11 010559" src="https://github.com/user-attachments/assets/bef7dd1e-f8f9-4d52-833e-ccb05ff424ff" />
<img width="1888" height="901" alt="Screenshot 2025-07-11 011315" src="https://github.com/user-attachments/assets/edf16642-c6b3-44ad-b087-326e5127fb19" />
<img width="1916" height="907" alt="Screenshot 2025-07-11 011538" src="https://github.com/user-attachments/assets/eafd53da-2de9-4229-b5eb-5fbb7422a1c6" />
<img width="1906" height="900" alt="Screenshot 2025-07-11 011810" src="https://github.com/user-attachments/assets/7f57e56a-cd1f-499c-a1ec-999e22323eba" />

<img width="800" height="691" alt="Screenshot 2025-07-11 060153" src="https://github.com/user-attachments/assets/50414110-cba6-43f5-9c99-c7cdb7df6ddb" />

<img width="1907" height="898" alt="Screenshot 2025-07-11 050421" src="https://github.com/user-attachments/assets/5cb45ce9-94d8-4b33-986e-f87a8444d6c6" />




	

⚙️ Step-by-Step Guide
1. Launch EC2 Instance
Type: t2.large (for Nexus memory usage)

AMI: Amazon Linux 2

Security Group: Allow 8081 (Nexus port), 22 (SSH)

2. Install Java and Nexus
bash
Copy
Edit
sudo yum install java-17-openjdk -y
# Download Nexus
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -xvf latest-unix.tar.gz
# Move and run
sudo mv nexus-* /opt/nexus
sudo useradd nexus
sudo chown -R nexus:nexus /opt/nexus
Start Nexus as a service (you can script or use systemd).

3. Setup Nexus Repository (on browser)
Go to http://<EC2-public-IP>:8081

Login with default creds (admin + password from admin.password file)

Create a Maven hosted repository named maven-releases or maven-snapshots

4. Update build.gradle
groovy
Copy
Edit
publishing {
    publications {
        mavenJava(MavenPublication) {
            from components.java
        }
    }
    repositories {
        maven {
            name = "Nexus"
            url = "http://<EC2-Public-IP>:8081/repository/maven-releases/"
            credentials {
                username = "admin"
                password = "yourpassword"
            }
        }
    }
}
5. Publish the Artifact
bash
Copy
Edit
./gradlew clean build publish
📁 Project Structure
css
Copy
Edit
java-app/
├── build.gradle
├── settings.gradle
├── src/main/java/...
├── gradle.properties
└── Dockerfile (optional)
🧠 Challenges Faced
Gradle's implicit dependency issue between bootJar and publish: resolved by adding dependsOn

Nexus initial setup was memory intensive — used t2.large

Cleaned up old Git history to make the repo fresh (rebased commits)

📌 Useful Commands
bash
Copy
Edit
# View old commits for a file
git log -- README.md

# Revert a file to an older commit
git checkout <commit-hash> -- README.md

# Rebase to clean history
git rebase --root -i


Documentation from Gradle and Sonatype Nexus

🔗 Connect
Feel free to check the LinkedIn post or star the repo if you found it helpful!
