mkdir -p ~/containers/radicale/data
mkdir -p ~/containers/radicale/config/config


podman run --rm -it \
  -v ~/containers/radicale/data:/data \
  docker.io/python:3-alpine \
  sh -c 'pip install bcrypt >/dev/null && python -c "
import bcrypt, getpass
pw = getpass.getpass(\"Password: \")
print(\"glen:\" + bcrypt.hashpw(pw.encode(), bcrypt.gensalt()).decode())
" >> /data/users'

cat ~/containers/radicale/data/users

Should see ...
glen:$2b$12$...

podman run -d \
  --name radicale \
  -p 5232:5232 \
  -v ~/containers/radicale/data:/data \
  -v ~/containers/radicale/config:/config:ro \
  docker.io/tomsquest/docker-radicale

Check it:
podman logs radicale
curl http://localhost:5232

nano ~/containers/radicale/config/config


