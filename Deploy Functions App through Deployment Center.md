

# Deploy Functions App through Deployment Center

1. register external git with http://shou7:< PAT >@github.dxc.com/scf-tnp/CAT.git
2. navigate http://FunctionsAppCD.scm.azurewebsites.net, go to Debug Console, 
3. ssh-keygen to generate a new ssh key
4. ssh-keyscan -t rsa github.dxc.com >> known_hosts
```
D:\home\.ssh>ssh-keyscan -t rsa github.dxc.com >> known_hosts
# github.dxc.com:22 SSH-2.0-babeld-d276e467
```
5. go to .ssh folder, open id_rsa.pub file, copy content
6. go to github -> setttings -> deploy key, paste the pub content
7. go back to scm portal, run: ssh -vT git@github.dxc.com  ( or ssh -vvv git@github.dxc.com )
```
  D:\home\.ssh>ssh -vT git@github.dxc.com
OpenSSH_8.3p1, OpenSSL 1.1.1g  21 Apr 2020
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: Connecting to github.dxc.com [34.202.75.143] port 22.
debug1: Connection established.
debug1: identity file /d/home/.ssh/id_rsa type 0
debug1: identity file /d/home/.ssh/id_rsa-cert type -1
debug1: identity file /d/home/.ssh/id_dsa type -1
debug1: identity file /d/home/.ssh/id_dsa-cert type -1
debug1: identity file /d/home/.ssh/id_ecdsa type -1
debug1: identity file /d/home/.ssh/id_ecdsa-cert type -1
debug1: identity file /d/home/.ssh/id_ecdsa_sk type -1
debug1: identity file /d/home/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file /d/home/.ssh/id_ed25519 type -1
debug1: identity file /d/home/.ssh/id_ed25519-cert type -1
debug1: identity file /d/home/.ssh/id_ed25519_sk type -1
debug1: identity file /d/home/.ssh/id_ed25519_sk-cert type -1
debug1: identity file /d/home/.ssh/id_xmss type -1
debug1: identity file /d/home/.ssh/id_xmss-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.3
debug1: Remote protocol version 2.0, remote software version babeld-d276e467
debug1: no match: babeld-d276e467
debug1: Authenticating to github.dxc.com:22 as 'git'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: rsa-sha2-512
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: Server host key: ssh-rsa SHA256:6eNrzLO+L4SGT+m6ZeQjlEAxkfD8ShaYl0FeFnbn5Lk
debug1: Host 'github.dxc.com' is known and matches the RSA host key.
debug1: Found key in /d/home/.ssh/known_hosts:1
Warning: Permanently added the RSA host key for IP address '34.202.75.143' to the list of known hosts.
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 134217728 blocks
debug1: Will attempt key: /d/home/.ssh/id_rsa RSA SHA256:x9rjz+p7fw9d2Kj30BqzHoHHrhE8fOVck9AnsQPz4kA
debug1: Will attempt key: /d/home/.ssh/id_dsa 
debug1: Will attempt key: /d/home/.ssh/id_ecdsa 
debug1: Will attempt key: /d/home/.ssh/id_ecdsa_sk 
debug1: Will attempt key: /d/home/.ssh/id_ed25519 
debug1: Will attempt key: /d/home/.ssh/id_ed25519_sk 
debug1: Will attempt key: /d/home/.ssh/id_xmss 
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-rsa,rsa-sha2-512,rsa-sha2-256,ssh-dss>

debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering public key: /d/home/.ssh/id_rsa RSA SHA256:x9rjz+p7fw9d2Kj30BqzHoHHrhE8fOVck9AnsQPz4kA
debug1: Server accepts key: /d/home/.ssh/id_rsa RSA SHA256:x9rjz+p7fw9d2Kj30BqzHoHHrhE8fOVck9AnsQPz4kA
debug1: Authentication succeeded (publickey).
Authenticated to github.dxc.com ([34.202.75.143]:22).
debug1: channel 0: new [client-session]
debug1: Entering interactive session.
debug1: pledge: network
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
Hi scf-tnp/CAT! You've successfully authenticated, but GitHub does not provide shell access.
debug1: channel 0: free: client-session, nchannels 1
Transferred: sent 3216, received 2324 bytes, in 0.0 seconds
Bytes per second: sent 66153.8, received 47805.2
debug1: Exit status 1
```

8. download publish profile file, and get user name and password from the file
9. in Github setting, register hooks: https://< username >:< password >@functionsappcd.scm.azurewebsites.net/deploy

# Q & A

1. Q: Host key verification failed

Host key verification failed.\r\nfatal: Could not read from remote repository.\n\nPlease make sure you have the correct access rights\nand the repository exists.\n\r\nD:\Program Files\Git\cmd\git.exe fetch origin --progress

A: 

> go to scm portal, check known_hosts and id_rsa, and id_rsa_pub files exist, if not so, run:
> ssh-keygen
> and then ssh-keyscan -t rsa github.dxc.com >> known_hosts

```
D:\home\.ssh>ssh-keyscan -t rsa github.dxc.com >> known_hosts
# github.dxc.com:22 SSH-2.0-babeld-d276e467
```

2. Q: Error: Permission denied (publickey)

A: 
> First, run ssh-keygen to generate new pub/private key pair, and register Deploy Key in github again
> If the issue still exists, Refer to [github](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/error-permission-denied-publickey#)


