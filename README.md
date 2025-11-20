# TODO
Do as much green check for the audit below? 

Support other linux distro ??

# How to work on this
There are a couple option, such as spinning up a VM. Change the inventory.ini user and IP according to the VM 
You should also set the password in inventory.ini, you can then run 
```
ansible-playbook playbook.yaml
```

after the security roles is executed, the SSH port will be change to whatever is configured (default is 2222)
also SSH via password will also no longer be able to be done, therefore do
```bash
ansible-playbook playbook.yaml -e "ansible_port=2222" 
```

To provision VM can also be done using Vagrant by doing
```bash
vagrant up --provider=libvirt
```

This VM can be destroyed by running
```bash
vagrant destroy -f
```



Another way is by running 
```bash
molecule test
```

Molecule can be installed via https://docs.ansible.com/projects/molecule/installation/#pip
If you do add a new roles, you can add them and follow the pattern in molecule/default/converge.yml


# Contribution
Contribute by making a new branch then push to main
To contribute to this project:  

1. **Create a New Branch**  
   ```bash
   git checkout -b your-branch-name
   ```

2. **Make Your Changes**  
   - Edit, add, or improve files as needed in your branch.

3. **Commit Your Changes**  
   ```bash
   git add .
   git commit -m "Describe your changes"
   ```

4. **Push Your Branch**  
   ```bash
   git push origin your-branch-name
   ```

5. **Create a Pull Request**  
   - Open a pull request to merge your branch into the main branch.  
   - Ensure your changes are reviewed and approved before merging.

**Note:** Always sync your branch with the main branch to avoid conflicts before merging.

# Audit
Also need to integrate this into the test or install automatically somehow, idk. Can add more if want
https://cisofy.com/lynis/controls/LYNIS/

```
sudo apt install lynis
sudo lynis audit system
```
or in
https://vpsaudit.vernu.dev/

```
curl -O https://raw.githubusercontent.com/vernu/vps-audit/main/vps-audit.sh && chmod +x vps-audit.sh && sudo ./vps-audit.sh
```