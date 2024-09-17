+++
title = 'Cronjobs'
date = 2024-09-13T12:58:16+02:00
draft = false
type="docs"
+++

Skate leverages systemd timers to schedule jobs.

```shell
cat <<EOF | skate apply -f -
apiVersion: batch/v1
kind: CronJob
metadata:
  name: test
  namespace: my-app
spec:
  schedule: "*/10 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Skate cluster
EOF
```

You can manually triger an already created cronjob:
```shell
skate create job -n my-app --from=cronjob/test burner
```