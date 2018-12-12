# Automating-release-validation-in-Fedora

I chose the TestCase Base Startup from the link. I automated everything using simple ansible plays. Here is the playbook :-

---<br>
- hosts: 192.168.122.83<br>
  tasks:<br><br>

  - name: reboot host<br>
    shell: 'sleep 2 && shutdown -r now'<br>
    async: 1<br>
    poll: 0<br>
    ignore_errors: yes<br><br><br>


  - name: Waiting for server to come back<br>
    become: false<br>
    local_action: wait_for host=192.168.122.83  port=22  search_regex=OpenSSH  state=started  delay=5<br><br><br>

  
  - name: Listing the enabled daemons ( confirming successful reboot )<br>
    command: systemctl list-unit-files | grep enabled<br><br><br>
    

  - name: Check success of enabled daemons<br>  
    command: echo $?<br>
