## GCP 사용 방법
### 1. tf 파일 작성
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
    } 
  } 
} 
```