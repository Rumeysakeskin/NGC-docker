# NGC-setup
NVIDIA GPU Cloud setup and building NVIDIA containers

If you get: 
```
ERROR: failed to solve: failed to fetch anonymous token: unexpected status: 401 Unauthorized
```
follow next steps:

- ### Install NGC CLI
[NVIDIA NGC CLI](https://ngc.nvidia.com/setup/installers/cli)

```python
root@79018b441af1:/workspace# ngc config set
Enter API key [no-apikey]. Choices: [<VALID_APIKEY>, 'no-apikey']: 
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Enter CLI output format type [ascii]. Choices: [ascii, csv, json]: 
Enter org [no-org]. Choices: ['xxxxxxxxxxxx']: xxxxxxxxxxxx
Enter team [no-team]. Choices: ['no-team']: no-team
Successfully saved NGC configuration to /root/.ngc/config
```

```python
root@79018b441af1:/workspace# ngc --version
NGC CLI 3.21.0
```



- ### Run Docker
- ### 
