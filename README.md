# Advance_Encryption_tools
import os
from cryptography.fernet import Fernet

def generate_key():
    """
    Generates a new encryption key and saves it to a file.
    """
    key = Fernet.generate_key()
    with open("secret.key", "wb") as key_file:
        key_file.write(key)

def load_key():
    """
    Loads the encryption key from the "secret.key" file.
    """
    return open("secret.key", "rb").read()

def encrypt_file(filename, key):
    """
    Encrypts a file using the provided key.
    """
    f = Fernet(key)
    with open(filename, "rb") as file:
        file_data = file.read()

    encrypted_data = f.encrypt(file_data)

    with open(filename + ".encrypted", "wb") as file:
        file.write(encrypted_data)

    print(f"File encrypted: {filename}.encrypted")

def decrypt_file(filename, key):
    """
    Decrypts a file using the provided key.
    """
    f = Fernet(key)
    with open(filename, "rb") as file:
        encrypted_data = file.read()

    try:
        decrypted_data = f.decrypt(encrypted_data)
    except Exception as e:  # Catch potential decryption errors
        print(f"Decryption failed: {e}")
        return

    with open(filename[:-10], "wb") as file:  # Remove ".encrypted" extension
        file.write(decrypted_data)

    print(f"File decrypted: {filename[:-10]}")


if __name__ == "__main__":
    if not os.path.exists("secret.key"):
        generate_key()

    key = load_key()

    import sys
    if len(sys.argv) == 4:
        mode = sys.argv[1]
        filename = sys.argv[2]
        password = sys.argv[3]  # You are not using this for encryption in this example

        if mode == "encrypt":
             encrypt_file(filename, key)
        elif mode == "decrypt":
            decrypt_file(filename, key)
        else:
            print("Invalid mode. Use 'encrypt' or 'decrypt'.")
    else:
        print("Usage: python fileencrypt.py [encrypt/decrypt] [filename] [password]")
        print("Note: The password is not used in this simple example.")
        print("It's best practice to use a strong key management system for real applications.")
