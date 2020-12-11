# JMulticrypt

Java Module for secure End-2-End encryption using the Multicrypt algorithm made by me.

## Concept behind MultiCrypt Algrorithm

So, take a number, say 77 as your message.
Then, take another number, say 739 as your key (In actual, this number will be of 256 bits).
Now, to get your encrypted message, just add your key to the actual message
i.e. 77 + 739 in this case = 816. So your encrypted message is now 816.

Now, encrypt your key i.e. 739 in this case using RSA Encryption (Asymmetric Encryption)
i.e. encrypt your key with the public key of the recipient, then append the encrypted key
at the end of your encrypted message with any seperator charecter between the encrypted message 
and the encrypted key.

Now this message is your final encrypyed message which can be transmitted safely to the
recipient.

Now when the recipient receives your encrypted message, then for decrypting that message first the
recipient will seperate the actual encrypted message and the encrypted key from the seperator charecter
which you used between the actual encrypted message and encrypted key at the time of encrypting the message
(In actual, the seperator charecter is fixed for everyone and is equal to "K"). Then the recipient will
decrypt the encrypted key using his/her private key which is mathematically linked to his/her public key.

Then the recipient gets the actual key i.e. 739 in this case. Then all he/she has to do is minus the actual key
from the actual encrypted message to get the actual message i.e. 816 - 739 in this case = 77 which is the actual message
which you wanted to send to the recipient.

This is highly secure as the encrypted message is safe from brute force attack. Also, no one can reverse engineer the
encryption algorithm to decrypt the message as the key required to do so is encrypted asymmetrically
(i.e. using RSA encryption algorithm) and everyone knows that RSA Encryption cannot be broken due to its public-private
key pair system.

Hence, MultiCrypt encrypiton is a hybrid encryption algorithm (i.e. involves both symmetric encryption algorithm (key encryption)
and asymmetric encryption (RSA encryption)) which is ideal for transmitting any kind of data securely using any kind of network protocol.

**Please do note that I am using a cutom RSA Encryption algorithm made by me for suiting the symmetric part of the MultiCrypt algorithm.
Hence, public and private keys of standard RSA Encryption System WILL NOT WORK in MultiCrypt Encryption System and vice versa.**

## Installation

Simply include the **JMulticrypt.jar** file in your project.

### Special Guide For Android Projects:

- Create **libs** folder in your **app** folder where your **app level build.gradle** file is present.
- Download **JMulticrypt.jar** from here, and place it in the **libs** folder created in previous step.
- In your **app level build.gradle** file, add the following line in **dependencies block**:

```gradle
implementation fileTree(dir: "libs", include: ["*.jar"])
```

- Done!

## Usage

Simply use this code (Modify according to your needs):

```java
import com.abhaytr.jmulticrypt.*;
import java.util.*;

class YourClass
{
  
  public static void main(String args[])
  {
    Map parameters = new HashMap(); //For optional parameters (More about them below).
    End2End e2e = new End2End(parameters);
    //Functions available for module specified below in "Functions" section.
  }

}
```

Optional Parameters that are available are listed below in Parameters section and have to be passed as a Map (keys as name of parameters listed in Parameters section and value is your desired choice according to the options available for that parameter as specified in the Parameters section) which will be the only parameter for End2End() constructor.

### Parameters

- **public_key (Optional):** Public Key to be used if you want to use existing key (Default: "").
- **private_key (Optional):** Private Key to be used if you want to use existing key (Default: "").
- **save (Optional):** Should be true/false. Specifies whether the keys have to be stored in a file or not (Default: true).
- **key_path (Optional):** Specifies the path and name of the file where the keys have to be stored, if save = true (Default: root of your java projct)**(NOTE: Default value won't work in Android Project)**.
- **new (Optional):** Should be true/false. Specifies whether it should ignore any existing key pairs and generate new key pair or not (Default: false).

## Functions

### keys()

```java
Map keys = e2e.keys();
```

Returns Private Key and Public Key in the form of Map of the format {"public": %YOUR_PUBLIC_KEY%, "private": %YOUR_PRIVATE_KEY%}.

### encrypt()

```java
String message = "Multicrypt algorithm is highly secure!";
String public_key = keys.get("public");
String encrypted_message = e2e.encrypt(message, public_key);
```

Encrypts the message using MULTICRYPT algorithm.

**Parameters**

- **message (Required):** Message to encrypt.
- **public_key (Required):** Public Key of the recipient of the message (for the asymmetric encryption part).


### decrypt()

```java
String private_key = keys.get("private");
String actual_message = e2e.decrypt(encrypted_message, private_key);
```

Decrypts the encrypted message using MULTICRYPT algorithm.

**Parameters**

- **message (Required):** Encrypted Message to decryt.
- **private_key (Optional):** Your Private Key required to decrypt any message which is encrypted with Public Key linked to that private key (Default: Key which was either passed in the parameters map or generated by the program for you).

Useful for transmitting data securely between 2 devices on a network.

