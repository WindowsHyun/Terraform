## GCP 사용 방법

### 1. 별도의 스냅샷이 있는경우 이미지 처리 작업
```
gcloud compute images create source-name \
--source-snapshot=source-snapshot-name \
--project=project-id
```
- source-name : 저장할 이미지 이름
  - ex) jenkins-slave-base-image
- source-snapshot-name : 스냅샷 이름
  - ex) jenkins-slave-server-latest
- project-id : GCP 프로젝트명

### 2. main.tf 파일 작성
```
# Google Cloud Provider 설정 
terraform { 
  required_providers { 
    google = { 
      source  = "hashicorp/google" 
      version = "~> 4.0" 
    } 
  } 
} 

# 변수 정의 
variable "project_id" { 
  type    = string 
  default = "stage" 
} 

variable "zone" { 
  type    = string 
  default = "us-west1-b" 
} 

variable "instance_name" { 
  type = string 
} 

# Provider 설정 
provider "google" { 
  project = var.project_id 
  region  = "us-west1" 
  zone    = var.zone 
} 

# Compute Engine 인스턴스 생성 
resource "google_compute_instance" "default" { 
  name         = var.instance_name 
  machine_type = "e2-medium" 
  zone         = var.zone 
  boot_disk { 
    initialize_params { 
      image = "jenkins-slave-base-image" 
      size  = 50 
    } 
  } 

  network_interface { 
    network = "default" 
    access_config { 
      // 외부IP 설정시 해당 구문을 넣어야 합니다.
    } 
  } 
}

firewall {
    name    = "allow-http"
    network = "default"
    allow {
      protocol = "tcp"
      ports    = ["8080"]
    }
    source_ranges = ["0.0.0.0/0"]
    target_tags = ["http-server"]
}
```