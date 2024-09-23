# Ansible-build et ansible-runner

Ansible-build permet de créer des images de container permettant d'exécuter Ansible.

Cela nécessite les fichiers suivants :

- execution-environment.yaml
- requirements.yaml (ansible)

[La documentation du format].(https://docs.ansible.com/automation-controller/latest/html/userguide/ee_reference.html#dependencies)

## Création de l'image

```bash
$ ansible-builder build --tag mon_env_ee
```

## Exécution

Soit le playbook (`playbooks/test.yaml`) suivant :

```yaml
- name: Gather and print local facts
  hosts: localhost
  become: true
  gather_facts: true
  tasks:

   - name: Print facts
     ansible.builtin.debug:
      var: ansible_facts
```

On exécute ce playbook avec `ansible-navigator`.

```bash
$ ansible-navigator run playbooks/test.yaml --execution-environment-image mon_env_ee --mode stdout --pull-policy missing                                    ─╯
```

Ou encore, avec `ansible-runner` : 

```bash
ansible-runner run ./ --inventory inventory --container-image mon_env_ee:latest -p playbooks/test.yaml
```

en utilisant le fichier `inventory`

```yaml
all:
  hosts:
    localhost
```

## Vérification

Il est possible d'explorer le contenu de l'image avec `ansible-navigator` avec `:images` et un appui rusé sur des touches.
