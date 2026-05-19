KEY_ID=$(aws kms create-key \
  --description "xfusion symmetric encryption key" \
  --key-usage ENCRYPT_DECRYPT \
  --customer-master-key-spec SYMMETRIC_DEFAULT \
  --query 'KeyMetadata.KeyId' \
  --output text)

### Create the required alias
aws kms create-alias \
  --alias-name alias/xfusion-KMS-Key \
  --target-key-id $KEY_ID

echo "Key created and aliased as: alias/xfusion-KMS-Key"
echo "Key ID: $KEY_ID"

aws kms encrypt \
  --key-id alias/xfusion-KMS-Key \
  --plaintext fileb:///root/SensitiveData.txt \
  --output text \
  --query CiphertextBlob \
  | base64 --decode > /root/EncryptedData.bin

### Decrypt to a temporary file
aws kms decrypt \
  --ciphertext-blob fileb:///root/EncryptedData.bin \
  --output text \
  --query Plaintext \
  | base64 --decode > /root/DecryptedData.txt

### Compare files (no output = identical)
diff /root/SensitiveData.txt /root/DecryptedData.txt

### Quick success message
[ $? -eq 0 ] && echo "Verification successful - files match" || echo "Mismatch detected"

### Clean up
rm -f /root/DecryptedData.txt
