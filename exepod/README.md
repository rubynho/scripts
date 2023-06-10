# exepod

The exepod script is just like the `kubectl exec` command, but it lets you choose a pod by an ID (inferring the container) instead of its full name.

### Usage

```bash
kubectl get pods -n anyspace
# NAME                                    READY   STATUS    RESTARTS      AGE
# pod-one-77c799b5b8-q9rcw                2/2     Running   0             24h
# pod-two-574f7897c8-vb2t7                2/2     Running   0             24h
# pod-three-5b649678cf-n9vbk              2/2     Running   0             24h

exepod 1 sh
# Getting into pod pod-one-77c799b5b8-q9rcw on container anycontainer
# / $

kubectl exec -ti pod-one-77c799b5b8-q9rcw -c anycontainer -- sh
# / $
```

The commands 2 and 3 are equivalent.

### Installation

```bash
# Download exepod file into a folder that will be added to $PATH
wget ~/.local/bin https://raw.githubusercontent.com/rubynho/scripts/main/exepod/exepod -P

# Make the script executable
chmod +x ~/.local/bin/exepod

# Add folder to path
echo 'export PATH=$PATH:$HOME/.local/bin' >> ~/.zshrc # or ~/.bashrc
```

To improve even more the experience, a new command can be defined to print an ID column along with the output of `kubectl get pods`.

```bash
kgp() {
  local COUNT=1

  kubectl get pods $@ | while read LINE; do
    if [[ "$LINE" =~ "NAME" ]]; then
      echo "ID   $LINE"
    else
      printf "%2i   " $COUNT
      echo $LINE
      ((++COUNT))
    fi
  done
}
```

The function above can be added to the .zshrc or .bashrc files to make it available in any shell session. The output will be:

```bash
kgp
# ID   NAME                                    READY   STATUS    RESTARTS      AGE
#  1   pod-one-77c799b5b8-q9rcw                2/2     Running   0             24h
#  2   pod-two-574f7897c8-vb2t7                2/2     Running   0             24h
#  3   pod-three-5b649678cf-n9vbk              2/2     Running   0             24h
```
