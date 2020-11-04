Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64" 
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
    libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
    xz-utils tk-dev libffi-dev liblzma-dev cifs-utils python-openssl git
    
    git config --global core.compression 9
    git clone https://github.com/pyenv/pyenv.git /home/vagrant/.pyenv 
    echo 'export PYENV_ROOT="/home/vagrant/.pyenv"' >> /home/vagrant/.profile
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> /home/vagrant/.profile
    echo 'eval "$(pyenv init -)"' >> /home/vagrant/.profile
    source /home/vagrant/.profile

    pyenv install 3.8.2
    pyenv global 3.8.2
  
    curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python 
    mv /root/.poetry /home/vagrant/.poetry
    chmod +x -R /home/vagrant/.poetry/bin
    echo 'export PATH="/home/vagrant/.poetry/bin:$PATH"' >> /home/vagrant/.profile
  SHELL
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.trigger.after :up do |trigger|
    trigger.name = "Launching App"
    trigger.info = "Running the TODO app setup Script"
    trigger.run_remote = {privileged: false, inline: "
      cd /vagrant
      poetry install
      poetry run flask run --host=0.0.0.0
    "}
  end
end