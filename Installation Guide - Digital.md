A simulation environment -[Github link](https://github.com/hneemann/Digital)

## Installation Process:

My plan is to create an isolated environment- similar to .venv in Python. Since it is a Java based software let's find out how to do it in Java environment.

### Step 1: Install Java Runtime Environment

```bash
arcticwolf@MacMan ~/D/R/simulation> curl -L -o jre.tar.gz 

arcticwolf@MacMan ~/D/R/simulation> mkdir jre && tar -xzf jre.tar.gz -C jre --strip-components=1

arcticwolf@MacMan ~/D/R/simulation> rm jre.tar.gz 

arcticwolf@MacMan ~/D/R/simulation> ./jre/Contents/Home/bin/java -version
```

Output:
![[Image_Assets/Pasted image 20261002003908.png]]

### Step 2: Download Digital
Get `Digital.zip` from [https://github.com/hneemann/Digital/releases](https://github.com/hneemann/Digital/releases), then:
```bash
unzip ~/Downloads/Digital.zip -d ~/Desktop/Repo/simulation/Digital/
sudo xattr -dr com.apple.quarantine ~/Desktop/Repo/simulation/Digital/
ls ~/Desktop/Repo/simulation/Digital/
```

Since I am using fish shell, here is how I set it up:
```bash
printf '%s\n' '#!/bin/bash' 'DIR="$(cd "$(dirname "$0")" && pwd)"' 'export JAVA_HOME="$DIR/jre/Contents/Home"' 'export PATH="$JAVA_HOME/bin:$PATH"' 'exec java -jar "$DIR/Digital/Digital.jar" "$@"' > ~/Desktop/Repo/simulation/run-digital.sh
chmod +x ~/Desktop/Repo/simulation/run-digital.sh
```
```bash
cat ~/Desktop/Repo/simulation/run-digital.sh

# Output

#!/bin/bash
DIR="$(cd "$(dirname "$0")" && pwd)"
export JAVA_HOME="$DIR/jre/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"
exec java -jar "$DIR/Digital/Digital.jar" "$@"
```

Now for the alias:
```bash
alias digital '~/Desktop/Repo/simulation/run-digital.sh'
funcsave digital
```

**`funcsave digital`**  
Saves that function to a file (`~/.config/fish/functions/digital.fish`). Without it, the alias only exists in the current terminal session and disappears when you close it. With it, fish loads it automatically in every new session.