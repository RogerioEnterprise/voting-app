# Infrastruktur Terraform

## Resource yang dibuat
- S3 Bucket: menyimpan artefak aplikasi dan image docker
- EC Instance: meng-host docker containers (t2.micro untuk efisiensi biaya)
- Security Group: mengontrol akses jaringan terhadap host Docker

## Penggunaan
```bash
cd terraform
terraform init
terraform plan
terraform apply
```

## Clean up
```bash
terraform destroy
```