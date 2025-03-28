# docker build -t vminh492018/ansible-ubuntu:1.0 -f Dockerfile .
FROM ubuntu:22.04

# Environment variables
ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONUNBUFFERED=1 \
    PYTHON_VERSION=3.10 \
    ANSIBLE_VERSION=2.15.13 \
    ANSIBLE_CONFIG=/ansible/ansible.cfg

# Cài đặt các gói cơ bản và cấu hình hệ thống
RUN apt-get update && apt-get install -y --no-install-recommends \
    software-properties-common \
    sudo \
    python3.10 \
    python3.10-venv \
    python3.10-dev \
    python3-pip \
    gcc \
    make \
    git \
    openssh-client \
    openssh-server \
    curl \
    gnupg \
    libyaml-dev \
    vim \
    sshpass \
    libssl-dev \
    libffi-dev \
    pkg-config \
    # MySQL/MariaDB requirements
    mysql-client \
    mariadb-client \
    libmysqlclient-dev \
    # Redis requirements
    redis-tools \
    # Network tools
    iputils-ping \
    traceroute \
    telnet \
    && update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1 \
    && update-alternatives --install /usr/bin/python python /usr/bin/python3.10 1 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Cài đặt Python dependencies
RUN pip3 install --no-cache-dir --upgrade pip && \
    pip3 install --no-cache-dir \
    "ansible-core==${ANSIBLE_VERSION}" \
    jinja2==3.1.2 \
    paramiko \
    PyMySQL \
    redis \
    certifi \
    cryptography \
    pymysql \
    mysqlclient

# Cài đặt Ansible collections
RUN ansible-galaxy collection install community.general community.mysql

# Tạo cấu hình mặc định cho Ansible để sử dụng PyMySQL
RUN mkdir -p /ansible && \
    echo "[defaults]" > /ansible/ansible.cfg && \
    echo "interpreter_python = /usr/bin/python3" >> /ansible/ansible.cfg && \
    echo "ansible_python_interpreter = /usr/bin/python3" >> /ansible/ansible.cfg && \
    echo "[mysql]" >> /ansible/ansible.cfg && \
    echo "connector = pymysql" >> /ansible/ansible.cfg

# Enable IPv4 forwarding
RUN echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf

WORKDIR /ansible
CMD ["/bin/bash"]