# Ansible Role: GitHub Users

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-github-users.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-github-users)

Create users based on GitHub accounts.

This role will take a GitHub username and create a system account with the same username, and will add all the pubkeys associated with the GitHub account to the user's `authorized_keys`.

It's kind of a cheap way to do public key management for users on your system... but it works!

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    github_users: []
      # You can specify an object with 'name' (required) and 'groups' (optional):
      # - name: geerlingguy
      #   groups: www-data,sudo
    
      # Or you can specify a GitHub username directly:
      # - geerlingguy

A list of users to add to the server; the username will be the `name` (or the bare list item, if it's a string instead of an object). You can add the user to one or more groups (in addition to the `[username]` group) by adding them as a comma-separated list in `groups`.

    github_users_absent: []
      # You can specify an object with 'name' (required):
      # - name: geerlingguy
    
      # Or you can specify a GitHub username directly:
      # - geerlingguy

A list of users who should _not_ be present on the server. The role will ensure these user accounts are removed.

## Dependencies

None.

## Example Playbook

    - hosts: servers
    
      vars:
        github_users:
          # You can specify the `name`:
          - name: geerlingguy
            groups: sudo,www-data
          - name: GrahamCampbell
          # Or if you don't need to override anything, you can specify the
          # GitHub username directly:
          - fabpot
    
        github_users_absent:
          - johndoe
          - name: josh
    
      roles:
        - geerlingguy.github-users

If you want to make sure users' public keys are in sync, it is best to run the playbook on a cron, e.g. every 5 min, 10 min, or some other interval. That way you don't have to manually add new keys for users.

## License

MIT / BSD

## Author Information

This role was created in 2017 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
