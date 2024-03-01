#!/bin/bash

echo Start a long-running process to keep the container pipes open
sleep infinity < /proc/1/fd/0 > /proc/1/fd/1 2>&1 &

echo Wait a bit before retrieving the PID
sleep 1

echo Save the long-running PID on file
echo $! > /container-pipes-pid

echo Start systemd as PID 1
exec /usr/lib/systemd/systemd

echo Attaching to pipes of PID `cat container-pipes-pid`
exec /bin/bash < /proc/`cat container-pipes-pid`/fd/0 > /proc/`cat container-pipes-pid`/fd/1 2>/proc/`cat container-pipes-pid`/fd/2

