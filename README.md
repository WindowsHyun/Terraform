## Terraform 설치 가이드 (Ubuntu/Debian)
이 가이드는 Ubuntu/Debian 시스템에 Terraform을 설치하는 방법을 안내합니다.

### 1. 시스템 패키지 업데이트
```
sudo apt update && sudo apt install -y gnupg software-properties-common
```

### 2. HashiCorp GPG Key 등록
```
wget -O- https://apt.releases.hashicorp.com/gpg |
gpg --dearmor |
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

### 3. GPG Fingerprint 확인 (선택)
```
gpg --no-default-keyring
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg
--fingerprint
```

### 4. HashiCorp Repository 추가
```
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg]
https://apt.releases.hashicorp.com $(lsb_release -cs) main" |
sudo tee /etc/apt/sources.list.d/hashicorp.list
```

### 5. Terraform 설치
```
sudo apt update && sudo apt install terraform
```

### 6. 설치 확인
```
terraform -v
```