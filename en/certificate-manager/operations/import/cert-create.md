# Creating a custom certificate

Let's look at creating a custom certificate using a self-signed certificate as an example. Custom certificate requirements are shown on the [{#T}](../../concepts/imported-certificate.md) page.

## Creating a self-signed certificate file {#create-file}

To create a self-signed certificate using the `OpenSSL` library, run the command below:

{% list tabs %}

- Bash

   ```bash
   openssl req -x509 -newkey rsa:4096 -nodes \
     -keyout key.pem \
     -out cert.pem \
     -days 365 \
     -subj '/CN=example.com'
   ```

- PowerShell

   ```PowerShell
   openssl req -x509 -newkey rsa:4096 -nodes `
     -keyout key.pem `
     -out cert.pem `
     -days 365 `
     -subj '/CN=example.com'
   ```

{% endlist %}

Where:
* `-x509`: Command will output a certificate file.
* `-newkey`: New private key file will be created.
* `rsa:4096`: Algorithm and key length.
* `-nodes`: Do not encrypt the private key file.
* `-out`: Certificate file name.
* `-days`: Certificate validity period.
* `-subj`: Certificate owner's Common Name value.

The `req` command launched with the above parameters will issue a self-signed certificate and generate the associated private key.

## Creating a self-signed user certificate {#create-certificate}

To create a user certificate:

{% list tabs %}

- Management console

   1. In the [management console]({{ link-console-main }}), select the folder to create a user certificate in.
   1. In the list of services, select **{{ certificate-manager-name }}**.
   1. Click **Add certificate**.
   1. In the menu that opens, select **User certificate**.
   1. In the window that opens, enter a name of your user certificate in the **Name** field.
   1. (Optional) Enter certificate description in the **Description** field.
   1. In the **Certificate** field, click **Add certificate**.
      1. Select **File** as the adding method.
      1. Click **Select file**.
         1. In the resulting window, select the `cert.pem` self-signed certificate file.
      1. Click **Add**.
   1. In the **Private key** field, click **Add private key**.
      1. Select **File** as the adding method.
      1. Click **Select file**.
         1. In the resulting window, select the `key.pem` private key file.
      1. Click **Add**.
   1. Click **Create**.

- CLI

   {% include [cli-install](../../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

   1. View a description of the command:

      ```bash
      yc certificate-manager certificate create --help
      ```

   1. Run this command:

      ```bash
      yc certificate-manager certificate create \
        --name mycert \
        --chain mycert.pem \
        --key mykey.pem
      ```

      Where:
      * `--name`: Certificate name.
      * `--chain`: Path to the certificate chain file.
      * `--key`: Path to the file of the certificate private key.

      Command result:

      ```bash
      id: fpqmg47avvimp7rvmp30
      folder_id: b1g7gvsi89m34qmcm3ke
      created_at: "2020-09-15T06:54:44.916325Z"
      ...
      issued_at: "2020-09-15T06:54:44.916325Z"
      not_after: "2021-09-15T06:48:26Z"
      not_before: "2020-09-15T06:48:26Z"
      ```

- {{ TF }}

   If you do not have {{ TF }} yet, [install it and configure the {{ yandex-cloud }} provider](../../../tutorials/infrastructure-management/terraform-quickstart.md#install-terraform).

   1. In the configuration file, describe the parameters of the resources you want to create:

      
      ```hcl
      resource "yandex_cm_certificate" "user-certificate" {
        name    = "<certificate_name>"

        self_managed {
          certificate = <<-EOT
                        -----BEGIN CERTIFICATE-----
                        <certificate_contents>
                        -----END CERTIFICATE-----
                        EOT
          private_key = <<-EOT
                        -----BEGIN PRIVATE KEY-----
                        <certificate_private_key_contents>
                        -----END PRIVATE KEY-----
                        EOT
        }
      }
      ```



      Where:

      * `name`: Certificate name.
      * `certificate`: Contents of the [certificate](../../concepts/imported-certificate.md) file.
      * `private_key`: Contents of the private key file.

      For more information about the `yandex_cm_certificate` resource parameters in {{ TF }}, see the [provider documentation]({{ tf-provider-link }}/cm_certificate).

   1. Create resources:

      {% include [terraform-validate-plan-apply](../../../_tutorials/terraform-validate-plan-apply.md) %}

   This will create a certificate in the specified folder. You can check if the certificate is there and properly configured either from the [management console]({{ link-console-main }}) or using this [CLI](../../../cli/quickstart.md) command:

    ```bash
    yc certificate-manager certificate get <certificate_name>
    ```

- API

   To create a certificate, use the [create](../../api-ref/Certificate/create.md) REST API method for the [Certificate](../../api-ref/Certificate/) resource or the [CertificateService/Create](../../api-ref/grpc/certificate_service.md#Create) gRPC API call.

{% endlist %}

A new certificate with the `Issued` status appears in the certificate list.