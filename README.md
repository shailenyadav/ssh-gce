#To set up a CI/CD pipeline using Google Compute Engine and GitHub Actions with an SSH key (without Docker), you can follow these steps:

1. **Create a Google Compute Engine instance**:
   - Log in to the Google Cloud Console.
   - Navigate to Compute Engine.
   - Click on "Create Instance" to create a new virtual machine.
   - Configure the instance according to your requirements, such as choosing the desired machine type, region, and SSH key.
   - Make sure to note down the instance's IP address.

2. **Generate an SSH key pair**:
   - Open your local terminal or command prompt.
   - Use the `ssh-keygen -t ecdsa -C "<your_comment>"` command to generate an SSH key pair.
   - Specify a name for the key pair and provide a passphrase (optional).
   - This command will create two files: a private key (`id_rsa`) and a public key (`id_rsa.pub`).

3. **Add the public SSH key to the Google Compute Engine instance**:
   - SSH into the Compute Engine instance using the IP address and the SSH key you provided during the instance creation.
   - Once connected, open the `~/.ssh/authorized_keys` file using a text editor.
   - Append the contents of the `id_rsa.pub` file (your public key) to the `authorized_keys` file.
   - Save and close the file.

4. **Configure GitHub Actions**:
   - In your GitHub repository, navigate to the "Settings" tab.
   - Select "Secrets" from the left-hand menu.
   - Click on "New repository secret" and give it a name (e.g., `SSH_PRIVATE_KEY`).
   - Open the `id_rsa` file (your private key) and copy its contents.
   - Paste the private key into the "Value" field of the new secret.
   - Click on "Add secret" to save it.

5. **Create a GitHub Actions workflow**:
   - In your GitHub repository, navigate to the "Actions" tab.
   - Click on "Set up a workflow yourself" or choose a template based on your requirements.
   - Define the workflow steps according to your build and deployment process.
   - Add the necessary steps to establish an SSH connection with the Compute Engine instance.

6. **Establish an SSH connection using GitHub Actions**:
   - Use the `ssh-action` GitHub Actions plugin to connect to the Compute Engine instance.
   - Include the following step in your workflow, replacing `<instance-ip>` with the IP address of your Compute Engine instance:
     ```
     name: SSH to Google Cloud Platform compute instances

      on:
        push:
          branches:
            - main
      
      jobs:
        deploy:
          runs-on: ubuntu-latest
          steps:
            - name: SSH to Google Cloud Platform compute instances
              uses: appleboy/ssh-action@master
              with:
                host: <instance_ip>
                port: <ssh_port>
                username: <key_user_name>
                key: ${{ secrets.SSH_PRIVATE_KEY }}
                instance-name: <instance_name>
                zone: <instance_location>
                script: |
                  cd ~
                  touch fromgithub.txt
     ```

7. **Commit and push your changes**:
   - Save the workflow file and commit it to your repository.
   - Push the changes to trigger the GitHub Actions workflow.
   - GitHub Actions will execute the defined steps, establishing an SSH connection to the Compute Engine instance and running the specified commands.

That's it! With these steps, you can set up a CI/CD pipeline using Google Compute Engine, GitHub Actions, and SSH key authentication without Docker. Remember to adjust the commands and configuration according to your specific needs and project requirements.
