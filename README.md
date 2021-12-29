# Задание 1. Яндекс.Облако (обязательное к выполнению)

подготовил конфиг для kms ключа и применил его в настройках бакета.

```terraform
resource "yandex_kms_symmetric_key" "key-for-bucket" {
  name              = "key-for-bucket"
  default_algorithm = "AES_256"
  rotation_period   = "8760h"
}

resource "yandex_storage_bucket" "desema-cloud-public-bucket" {
  bucket = "desema-cloud-public-bucket"
  acl    = "public-read"

  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        kms_master_key_id = yandex_kms_symmetric_key.key-for-bucket.id
        sse_algorithm     = "aws:kms"
      }
    }
  }
}
```