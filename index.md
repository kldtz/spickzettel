---
title: Spickzettel 
---

## BIOS

```bash
# Install utility
apt install smbios-utils
# Display supported thermal options
smbios-thermal-ctl -i
# Display thermal info of system
smbios-thermal-ctl -g
# Set thermal mode (balanced, cool-bottom, quiet, performance)
smbios-thermal-ctl --set-thermal-mode=quiet

# Dump computer's DMI (SMBIOS) table contants in human-readable form
# (description of hardware components, serial number, BIOS revision etc.)
dmidecode
# Only show info for DMI type 1 (System, see man pages for table of 42 types)
dmidecode -t 1
```

## Cargo

```bash
# Compile and run in release mode
cargo run --release
# Watch source files, execute 'run' on changes
cargo watch -x run
# Build package docs, open in browser
cargo doc --open
```

## Compression

```bash
# Peek at first few lines of gz file
gzip -cd news.2014.ru.shuffled.gz | head
```


## Conda 

```bash
# Create new environment that contains Python 3.8 and pip
conda create --name ENV-NAME python=3.8 pip
# Install package in environment
conda install -n ENV-NAME PACKAGE-NAME
# Remove environment
conda remove --name ENV-NAME --all

# (De)activate environment
conda activate ENV-NAME
conda deactivate ENV-NAME
```


## Copying Files

```bash
# Source dir with trailing slash: copy contents
rsync -r source/ target # => target/*
# Source dir without slash: copy source itself into target
rsync -r source target # => target/source/*
# Backup (preserving symbolic links, permissions, times, ownsership;
# deleting files in the target that no longer exist in the source)
rsync -av --delete source/ target/ 
```

## Database

```bash
# Log into postgres as postgres user
sudo -u postgres psql
# Set password for user
ALTER USER postgres PASSWORD 'password';
# Exit psql client
\q
```


## Docker


```bash
# Get running container from tagged image and run bash interactively
# -i connects STDIN of command inside container to STDIN of docker run/exec
# -t input is a terminal (allocate pseudo tty)
docker run -it image-name:1.0.0 bash
# Connect to running container, execute bash
docker exec -it container-id bash

# Copy file from host into running Docker container
docker cp file.txt d90d46a905df:/path/inside/container/file.txt
# Copy directory from container to host
docker cp <container-hash>:/dir/path/inside/container/ ~/path/on/host/
```

### Docker Machine

```bash
# Import machine from ZIP file
machine-import machine-name.zip
# Connect via SSH to specified machine
docker-machine ssh machine-name
# Set env variables to make docker run against specified machine
docker-machine env machine-name

```

### Docker Compose

```bash
# Stop, remove, build, launch container
docker-compose stop container-name
docker-compose rm container-name
docker-compose build container-name
docker-compose up container-name

# Remove all stopped service containers
docker-compose rm

# Build image without cache
docker-compose build --no-cache bot-trup

# View log files of specific container (by name defined in yml)
# using docker-compose.yml inside current dir
docker-compose logs container-name
# using docker-compose-custom.yml
docker-compose -f docker-compose-custom.yml logs container-name
```


## Encryption

```bash
# Create SSH key pair: key type RSA, 2048 bits, with comment
ssh-keygen -t rsa -b 2048 -C "<user-id>"
# Tell ssh to use specified key
ssh -i /path/to/private/key user@host


# Start ssh-agent (evaluate shell commands required to set up environment variables 
# printed at startup: SSH_AUTH_SOCK, SSH_AGENT_PID)
eval $(ssh-agent -s)
# List fingerprints of all identities
ssh-add -l
# Add private key identity
ssh-add /path/to/private/key
# Export base64-encoded key to env variable (encode to remove newlines)
export VARIABLE_NAME=$(cat /path/to/your/private/key | base64 -w0)
# Decode private key from env variable and add it to ssh-agent
ssh-add <(echo $GITLAB_PRIV_KEY | base64 --decode)

# Generate secure random secret key
openssl rand -hex 32
```


## Git

```bash
# Rename current local branch
git branch -m <new-name>
# Rename specified local branch 
git branch -m <old-name> <new-name>

# Clean untracked files
# (-n dry run, -d recurse into untracked dirs)
git clean

# Display remote URL
git remote -v
# Set remote URL
git remote set-url origin git@github.com:<user>/<repo>.git
```

### LFS

```bash 
# Install client via apt
apt install git-lfs
# Set up git-lfs in Git config
git lfs install
# Track large files via pattern
# (patterns will be written to .gitattributes)
git lfs track '*.xml'
# List all tracked patterns
git lfs track
```

### Comparing and Searching

```bash
# Compare dir/file between master and branch
git diff master:src/dir branch:src/dir
git diff master:src/dir/file branch:src/dir/file

# Search commit by commit metadata
git log --all --grep="<pattern>"
# Search commit contents
git grep "<pattern>" $(git rev-list --all)

# Follow changes of single file
gitk --follow path/to/file
```

### Submodules

```bash
# Create submodule under different name
git submodule add git@... dir_name/
# Initialize submodules from .gitmodules
git submodule init
# Update submodules to what superproject expects
git submodule update
# Execute "git pull" in all submodules
git submodule foreach "git pull"
# Execute "git pull" in all submodules recursively
git submodule foreach --recursive "git pull"
```


## Image Processing

```bash
# Install ImageMagick
apt install imagemagick
# Resize image relative to current size
convert -resize 50% image.jpg resized-image.jpg
# Convert to grayscale image
convert image.jpg -colorspace Gray image-gray.jpg
# Convert to binary image
convert image.jpg -monochrome image-bw.jpg

# Compress PNG
# Install pngquant
apt install pngquant
# Run default compression on all files (performance seems better than optipng)
pngquant *

# Convert image to PDF (make sure to set necessary rights in 
# /etc/ImageMagick-7/policy.xml if permission error occurs)
convert source.jpeg -auto-orient target.pdf
# Compress PDF
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=source.pdf target.pdf
```


## Monitoring

```bash
# Monitor processes (tree view)
htop -t
# Show human-readable disk usage summary sorted by size in reverse order
du -sh * | sort -rh
# Monitor GPU usage (Nvidia) with changes highlighted
watch -d -n 0.5 nvidia-smi
# Monitor temperature of CPUs etc.
apt install lm-sensors
watch -d -n 0.5 sensors
```

## Networking

```bash
# Traceroute with ICMP instead of UDP
traceroute -I www.some-url.com

# Check for listening ports
# -i selects all Internet and X.25 network files
# -P no conversion of port numbers to port names for network files (e.g., 443 -> https; faster)
# -n no conversion of network numbers to host names (show IP instead of host name; faster)
lsof -i -P -n | grep LISTEN

# Check IP address
ip a
```

## Ownership and Permissions
```bash
# Show permissions set on file (or dir)
ls -l file.txt

# Add existing user to existing group
# (only takes effect after new login!)
usermod -a -G <group> <user>

# Remove read and write permissions for group and others
chmod go-rw file.txt
# Remove all permissions from file
chmod a-rwx file.txt
# Add read and write permissions for user who owns file
chmod u+rw file.txt
# Set read permissions for everybody, write for group and owner, execute access only for owner
# (r--=100=4, -w-=010=2, --x=001=1, ---=000=0) => uneven number means exec permission
chmod 764 file.txt

# Change permissions for entire dir tree (recursively)
# (user: all permissions, group and others: read, exec)
chmod -R 755 dir
# Remove exec permission on dir for others (= deny permission to search through dir)
chmod o-x dir
```


## Search

### Files and Dirs by Name

```bash
# Find files or directories
find . -type f -name "*.txt"
find . -type d -name "*.txt"
# Find empty files matching pattern, descend at most 1 level of directories
find . -type f -name "*.txt" -maxdepth 1 -size 0
# Find files matching pattern, modified on Feb. 20th, 2017
find . -name "*.txt" -type f -newermt 2017-02-20 -not -newermt 2017-02-21 -ls
# Iterate over find output
find . -name "*.txt" -type f | while read line; do
	name = $line
done
```

### (Files by) Text Content

```bash
# Find pattern in Julia files under current dir, exclude .git
grep -r "pattern" --include "*.jl" --exclude .git .
# Count number of different ANN files with rows containing more than 3 tab-separated columns
awk 'BEGIN {FS="\t"}; NF > 3 {print FILENAME}' *.ann | sort | uniq | wc -l
```


## Pip

```bash
# List all packages of current environment
pip freeze > requirements.txt
# List only packages used in project (current dir)
pipreqs .
# Install requirements (in order defined in file)
pip install -r requirements.txt
```


### Search and Replace

```bash
# Search and replace all occurence of REGEX-PATTERN with STRING in place
sed -i 's/REGEX-PATTERN/STRING/g' FILE-NAME
```

## Rbenv

```bash
# Install Ruby version 2.7.0
rbenv install 2.7.0
# Set version as global default
rbenv global 2.7.0
```


## Requests

```bash
# Set header 
curl -H "X-Header: value" http://localhost:3000 
# HTTP methods
# POST JSON payload
curl -X POST -H "Content-Type: application/json" http://localhost:3000 -d '{"key": "value"}'
# POST form-urlencoded payload
curl -X POST http://localhost:3000 -d "param1=value1&param2=value2"
# POST JSON from data file, pretty print response
curl -d "@data.json" -X POST http://localhost:3000 | jq '.'
```

## Shell

```bash
# Change login shell to fish
chsh -s `which fish`

# Create soft/symbolic link
ln -s <file_path> <link_name>
```


## Virtualenv

```bash
# Create stand-alone virtualenv
virtualenv --no-site-packages -p python3 ENV-NAME
# Activate environment
source /PATH/TO/ENV-NAME/bin/activate
# Deactivate current environment
deactiveate
```


## Yarn

```bash  
# Install dependencies from package.json
yarn
# Run script defined in package.json
yarn run SCRIPT
```
