Askisi 1

a)ekana to ask2_1.yaml kai meta egrapsa tin entoli kubectl apply -f ask2_1.yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: ask21
    spec:
    containers:
    - name: nginx
        image: nginx:1.23.3-alpine
        ports:
        - containerPort: 80
        name: http
        protocol: TCP

b)egrapsa kubectl port-forward ask21 8080:80 kai meta mpika sto http://localhost:8080/ opou m emfanise to site "Welocome to nginx"

c)patisa kubectl logs ask21 kai evgale

    /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
    /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
    10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
    10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
    /docker-entrypoint.sh: Configuration complete; ready for start up
    2023/03/27 17:57:07 [notice] 1#1: using the "epoll" event method
    2023/03/27 17:57:07 [notice] 1#1: nginx/1.23.3
    2023/03/27 17:57:07 [notice] 1#1: built by gcc 12.2.1 20220924 (Alpine 12.2.1_git20220924-r4)
    2023/03/27 17:57:07 [notice] 1#1: OS: Linux 5.10.16.3-microsoft-standard-WSL2
    2023/03/27 17:57:07 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
    2023/03/27 17:57:07 [notice] 1#1: start worker processes
    2023/03/27 17:57:07 [notice] 1#1: start worker process 31
    2023/03/27 17:57:07 [notice] 1#1: start worker process 32
    2023/03/27 17:57:07 [notice] 1#1: start worker process 33
    2023/03/27 17:57:07 [notice] 1#1: start worker process 34
    2023/03/27 17:57:07 [notice] 1#1: start worker process 35
    2023/03/27 17:57:07 [notice] 1#1: start worker process 36
    2023/03/27 17:57:07 [notice] 1#1: start worker process 37
    2023/03/27 17:57:07 [notice] 1#1: start worker process 38
    2023/03/27 17:57:07 [notice] 1#1: start worker process 39
    2023/03/27 17:57:07 [notice] 1#1: start worker process 40
    2023/03/27 17:57:07 [notice] 1#1: start worker process 41
    2023/03/27 17:57:07 [notice] 1#1: start worker process 42
    2023/03/27 17:57:07 [notice] 1#1: start worker process 43
    2023/03/27 17:57:07 [notice] 1#1: start worker process 44
    2023/03/27 17:57:07 [notice] 1#1: start worker process 45
    2023/03/27 17:57:07 [notice] 1#1: start worker process 46
    127.0.0.1 - - [27/Mar/2023:18:03:26 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36" "-"
    2023/03/27 18:03:26 [error] 32#32: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8080", referrer: "http://localhost:8080/"
    127.0.0.1 - - [27/Mar/2023:18:03:26 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:8080/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36" "-"


d)egrapsa kubectl exec -it ask21 -- /bin/sh kai mpika ston fakelo usr/share/nginx/html kai allaksa to index.html kai meta patisa exit

e)egrapsa kubectl cp ask21:usr/share/nginx/html/index.html selida.html kai tin anevasa ksana me kubectl cp selida.html ask21:usr/share/nginx/html/index.html

f)kubectl delete pod ask21


Askisi 2

kubectl apply -f ask2_2.yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
    name: ask22
    spec:
    template:
        spec:
        restartPolicy: Never
        containers:
            - name: ubuntu
            image: ubuntu:20.04
            command: ["/script/sample.sh"]
            volumeMounts:
                - name: script
                mountPath: "/script"
        volumes:
            - name: script
            configMap:
                name: ask22configmap
                defaultMode: 0511
    ---
    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: ask22configmap
    data:
    sample.sh: |
        #!/bin/bash
        apt-get update
        apt-get install git -y
        apt-get install make -y
        apt-get install -y wget
        wget https://github.com/gohugoio/hugo/releases/download/v0.111.3/hugo_extended_0.111.3_linux-amd64.deb
        dpkg --install hugo_extended_0.111.3_linux-amd64.deb
        git clone "https://github.com/chazapis/hy548"
        cd hy548 && git submodule init && git submodule update && make all
        rm -r /script/*
        cp ./html/public/* /script -R

gia na do an egine completed ekana to kubectl describe job ask22 kai m evgale afto
    Name:             ask22
    Namespace:        default
    Selector:         controller-uid=515548c3-e09d-45cb-9cad-4e597adca345
    Labels:           controller-uid=515548c3-e09d-45cb-9cad-4e597adca345
                    job-name=ask22
    Annotations:      batch.kubernetes.io/job-tracking:
    Parallelism:      1
    Completions:      1
    Completion Mode:  NonIndexed
    Start Time:       Wed, 29 Mar 2023 22:28:30 +0300
    Completed At:     Wed, 29 Mar 2023 22:30:20 +0300
    Duration:         110s
    Pods Statuses:    0 Active (0 Ready) / 1 Succeeded / 0 Failed
    Pod Template:
    Labels:  controller-uid=515548c3-e09d-45cb-9cad-4e597adca345
            job-name=ask22
    Containers:
    ubuntu:
        Image:      ubuntu:20.04
        Port:       <none>
        Host Port:  <none>
        Command:
        /script/sample.sh
        Environment:  <none>
        Mounts:
        /script from script (rw)
    Volumes:
    script:
        Type:      ConfigMap (a volume populated by a ConfigMap)
        Name:      ask22configmap
        Optional:  false
    Events:
    Type    Reason            Age    From            Message
    ----    ------            ----   ----            -------
    Normal  SuccessfulCreate  9m47s  job-controller  Created pod: ask22-zzncq
    Normal  Completed         7m57s  job-controller  Job completed


Askisi 3

gia na ginei h epikoinonia ekana ena PersistentVolume kai ena PersistentVolumeClaim oste ola ta containers na exoun kapoio shared xoro opou tha mpouroun na epikoinonisoun kai na afisoun ta files gia na ta parei kapoio allo container

    apiVersion: v1
    kind: PersistentVolume
    metadata:
    name: persistent-volume
    labels:
        type: local
    spec:
    storageClassName: ""
    capacity:
        storage: 4Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: "/mnt/data"

    ---

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
    name: pvcfolder
    spec:
    storageClassName: ""
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
        storage: 2Gi

    ---

    apiVersion: v1
    kind: Pod
    metadata:
    name: ask23pod
    spec:
    containers:
    - name: nginx
        image: nginx:1.23.3-alpine
        imagePullPolicy: Always
        volumeMounts:
        - name: script
        mountPath: /usr/share/nginx/html
        ports:
        - containerPort: 80
        name: http
        protocol: TCP
    volumes:
        - name: script
        persistentVolumeClaim:
            claimName: pvcfolder
    ---

    apiVersion: batch/v1
    kind: Job
    metadata:
    name: ask23job
    spec:
    template:
        spec:
        restartPolicy: Never
        containers:
        - name: ubuntu
            image: ubuntu:20.04
            imagePullPolicy: Always
            command: ["/build-script/sample.sh"]
            volumeMounts:
            - name: build-script
            mountPath: "/build-script"
            - name: script
            mountPath: "/script"
        volumes:
        - name: build-script
            configMap:
            name: ask23configmap
            defaultMode: 0511
        - name: script
            persistentVolumeClaim:
            claimName: pvcfolder

    ---

    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: ask23configmap
    data:
    sample.sh: |
        #!/bin/bash
        apt-get update
        apt-get install git -y
        apt-get install make -y
        apt-get install -y wget
        wget https://github.com/gohugoio/hugo/releases/download/v0.111.3/hugo_extended_0.111.3_linux-amd64.deb
        dpkg --install hugo_extended_0.111.3_linux-amd64.deb
        git clone "https://github.com/chazapis/hy548"
        cd hy548 && git submodule init && git submodule update && make all
        rm -r /script/*
        cp ./html/public/* /script -R

    ---

    apiVersion: batch/v1
    kind: CronJob
    metadata:
    name: hy548-refresh
    spec:
    schedule: "15 2 * * *"
    jobTemplate:
        spec:
        template:
            spec:
            restartPolicy: OnFailure
            containers:
                - name: hy548-refresh
                image: ubuntu:20.04
                command: ["/bin/bash"]
                args:
                    - "-c"
                    - |
                    if [[ "$(date +%s -d $(curl -s -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/chazapis/hy548 | jq -r '.pushed_at'))" -gt "$(date +%s -d 'yesterday 02:15')" ]]; then
                        apt-get update
                        apt-get install git -y
                        apt-get install make -y
                        apt-get install -y wget
                        wget https://github.com/gohugoio/hugo/releases/download/v0.111.3/hugo_extended_0.111.3_linux-amd64.deb
                        dpkg --install hugo_extended_0.111.3_linux-amd64.deb
                        git clone "https://github.com/chazapis/hy548"
                        cd hy548 && git submodule init && git submodule update && make all
                        rm -r /script/*
                        cp ./html/public/* /script -R
                    else
                        echo "GitHub repository has not been updated since yesterday at 2:15"
                    fi
                volumeMounts:
                    - name: script
                    mountPath: /script
            volumes:
                - name: script
                persistentVolumeClaim:
                    claimName: pvcfolder


Askisi 4

gia na do oti etrekse ekana kubectl port-forward ask24deployment-6c6b7fb7d7-7nccw(to tuxaio onoma pou edose) 8080:80

    apiVersion: v1
    kind: PersistentVolume
    metadata:
    name: persistent-volume
    labels:
        type: local
    spec:
    storageClassName: ""
    capacity:
        storage: 4Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: "/mnt/data"

    ---

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
    name: pvcfolder
    spec:
    storageClassName: ""
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
        storage: 2Gi

    ---

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: ask24deployment
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: ask24
    template:
        metadata:
        labels:
            app: ask24
        spec:
        containers:
        - name: nginx
            image: nginx:1.23.3-alpine
            ports:
            - containerPort: 80
            name: http
            protocol: TCP
            volumeMounts:
            - name: script
            mountPath: "/usr/share/nginx/html"
        initContainers:
        - name: ask24container
            image: ubuntu:20.04
            volumeMounts:
            - name: script
            mountPath: "/usr/share/nginx/html"
            command: ["/bin/sh"]
            args:
            - "-c"
            - |
                until [ -f /usr/share/nginx/html/index.html ]; do sleep 5; done
        volumes:
        - name: script
            persistentVolumeClaim:
            claimName: pvcfolder


    ---

    apiVersion: batch/v1
    kind: Job
    metadata:
    name: ask23job
    spec:
    template:
        spec:
        restartPolicy: Never
        containers:
        - name: ubuntu
            image: ubuntu:20.04
            imagePullPolicy: Always
            command: ["/build-script/sample.sh"]
            volumeMounts:
            - name: build-script
            mountPath: "/build-script"
            - name: script
            mountPath: "/script"
        volumes:
        - name: build-script
            configMap:
            name: ask23configmap
            defaultMode: 0511
        - name: script
            persistentVolumeClaim:
            claimName: pvcfolder

    ---

    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: ask23configmap
    data:
    sample.sh: |
        #!/bin/bash
        apt-get update
        apt-get install git -y
        apt-get install make -y
        apt-get install -y wget
        wget https://github.com/gohugoio/hugo/releases/download/v0.111.3/hugo_extended_0.111.3_linux-amd64.deb
        dpkg --install hugo_extended_0.111.3_linux-amd64.deb
        git clone "https://github.com/chazapis/hy548"
        cd hy548 && git submodule init && git submodule update && make all
        rm -r /script/*
        cp ./html/public/* /script -R

    ---

    apiVersion: batch/v1
    kind: CronJob
    metadata:
    name: hy548-refresh
    spec:
    schedule: "15 2 * * *"
    jobTemplate:
        spec:
        template:
            spec:
            restartPolicy: OnFailure
            containers:
                - name: hy548-refresh
                image: ubuntu:20.04
                command: ["/bin/bash"]
                args:
                    - "-c"
                    - |
                    if [[ "$(date +%s -d $(curl -s -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/chazapis/hy548 | jq -r '.pushed_at'))" -gt "$(date +%s -d 'yesterday 02:15')" ]]; then
                        apt-get update
                        apt-get install git -y
                        apt-get install make -y
                        apt-get install -y wget
                        wget https://github.com/gohugoio/hugo/releases/download/v0.111.3/hugo_extended_0.111.3_linux-amd64.deb
                        dpkg --install hugo_extended_0.111.3_linux-amd64.deb
                        git clone "https://github.com/chazapis/hy548"
                        cd hy548 && git submodule init && git submodule update && make all
                        rm -r /script/*
                        cp ./html/public/* /script -R
                    else
                        echo "GitHub repository has not been updated since yesterday at 2:15"
                    fi
                volumeMounts:
                    - name: script
                    mountPath: /script
            volumes:
                - name: script
                persistentVolumeClaim:
                    claimName: pvcfolder

    ---

    apiVersion: v1
    kind: Service
    metadata:
    name: ask24service
    spec:
    selector:
        app: ask24deployment
    ports:
    - name: http
        port: 8080
        targetPort: 8080
    type: ClusterIP