# Test using infracost in GCP


## Usage
URL: https://dashboard.infracost.io

1. Create account
2. Preapare and send a pull request
2.1 Open your repo on GitHub
2.2 Create a new branch called "infracost-test"
2.3 Create a file called infracost_test.tf in the repo

For example:

 provider "google" {
  region = "us-central1"
  project = "test"
}

resource "google_compute_instance" "my_instance" {
  zone = "us-central1-a"
  name = "test"

  machine_type = "n1-standard-16" # <<<<<<<<<< Try changing this to n1-standard-32 to compare the costs
  network_interface {
    network = "default"
    access_config {}
  }

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  scheduling {
    preemptible = true
  }

  guest_accelerator {
    type = "nvidia-tesla-t4" # <<<<<<<<<< Try changing this to nvidia-tesla-p4 to compare the costs
    count = 4
  }

  labels = {
    environment = "production"
    service = "web-app"
  }
}

resource "google_cloudfunctions_function" "my_function" {
  runtime = "nodejs20"
  name = "test"
  available_memory_mb = 512

  labels = {
    environment = "Prod"
  }
}

2.4 Commit the changes and create a pull request by clicking on Contribute and Open pull request

