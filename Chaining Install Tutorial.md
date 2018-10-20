### Chaining K8s Playground

1. install K8s playground
  ```
  $ git clone https://github.com/redtree0/k8sPlayground-HLF.git
  $ cd k8sPlayground-HLF
  $ pip install virtualenv
  $ virtualenv -p python3 k8s_env
  $ source k8s_env/bin/activate
  ```

2. virtual environment run
  ```
  $ pip install -r requirements.txt
  ```

3. install ansible role
  ```
  $ ansible-galaxy install -r requirements.yml -p roles/
  ```
  
4. create virtual machine
  ```
  $ vagrant up
  
  $ make cluster
  ```

5. check k8s state
  ```
  $ vagrant ssh kubemaster
  ```
  * check pod
    ```
    $ kubectl get pod -o wide
    ```
    ![image](https://user-images.githubusercontent.com/7833598/47254881-ad2d0780-d4a2-11e8-811d-16c9c162fd81.png)

  * check node
    ```
    $ kubectl get node -o wide
    ```
    ![image](https://user-images.githubusercontent.com/7833598/47254889-c7ff7c00-d4a2-11e8-8b6f-5f3597718008.png)

6. install geth (in vagrant ssh kubemaster)
  ```
    $ git clone https://github.com/redtree0/kuberneteth.git
    $ cd kuberneteth
  ```

7. install needs setting
  ```
    $ sudo apt-get install ruby -y
  ```

8. make deploy.yaml and deploy
  ```
    $ ./kuberneteth
    $ kubectl apply -f deployment.yaml
  ```
  
9. port forwarding
  ```
  $ sudo iptables -t nat -A PREROUTING -p tcp --dport 8545 -j DNAT --to-destination 127.0.0.1:8545

  $ kubectl get pod // namespace

  $ kubeclt port-forward geth-miner-deployment-~~ 8545
  ```
  
10. install geth
  ```
    $ cd ..
    $ git clone https://github.com/ethereum/go-ethereum
    $ cd go-ethereum
    $ sudo apt-get install -y build-essential golang
  ```
  
11. attach geth
  ```
    $ kubectl get pod -o wide
    // copy mining node virtual address
    $ ./build/bin/geth attach http://paste mining node virtual address:8545
    (ex, ./build/bin/geth attach http://10.244.2.4:8545)
  ```
  
12. miner start
  ```
    $ miner.start()
  ```
  
13. mining log check (new vagrant ssh kubemaster)
  ```
  // copy mining node virtual name
    $ kubectl log geth-miner-deployment-~~ -f 
    (ex, kubectl log geth-miner-deployment-66fbd7cc5f-9q555)
  ```
  
