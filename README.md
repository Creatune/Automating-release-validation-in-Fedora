# Automating-release-validation-in-Fedora

I chose the TestCase Base Startup from the link. I automated everything using simple ansible plays. Here is the playbook :-

---
- hosts: 192.168.122.83
  tasks:

  - name: reboot host
    shell: 'sleep 2 && shutdown -r now'
    async: 1
    poll: 0
    ignore_errors: yes


  - name: Waiting for server to come back
    become: false
    local_action: wait_for host=192.168.122.83  port=22  search_regex=OpenSSH  state=started  delay=5

  
  - name: Listing the enabled daemons ( confirming successful reboot )
    command: systemctl list-unit-files | grep enabled
    

  - name: Check success of enabled daemons  
    command: echo $?
