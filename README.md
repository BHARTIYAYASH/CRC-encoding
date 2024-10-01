# CRC-encoding
CRC Encoding and Transmission Error Detection using Python  This project implements the **Cyclic Redundancy Check (CRC)** algorithm in Python for detecting transmission errors. It simulates both the **sender** and **receiver** sides, ensuring that the data transmitted is error-free.


class CRC:

    def __init__(self):
        self.cdw = ''

    # Function to perform XOR between two binary strings
    def xor(self, a, b):
        result = []
        for i in range(1, len(b)):
            if a[i] == b[i]:
                result.append('0')
            else:
                result.append('1')
        return ''.join(result)

    # Function to perform the CRC algorithm on the message with the given key
    def crc(self, message, key):
        pick = len(key)
        tmp = message[:pick]

        while pick < len(message):
            if tmp[0] == '1':
                tmp = self.xor(key, tmp) + message[pick]
            else:
                tmp = self.xor('0' * pick, tmp) + message[pick]
            pick += 1

        if tmp[0] == "1":
            tmp = self.xor(key, tmp)
        else:
            tmp = self.xor('0' * pick, tmp)

        checkword = tmp
        return checkword

    # Function to encode the data on the sender's side
    def encodedData(self, data, key):
        l_key = len(key)
        append_data = data + '0' * (l_key - 1)
        remainder = self.crc(append_data, key)
        codeword = data + remainder
        self.cdw = codeword
        print("Remainder: ", remainder)
        print("Data after appending CRC (codeword): ", codeword)

    # Function to simulate receiver side for checking if any error exists
    def receiverSide(self, key, data):
        r = self.crc(data, key)
        if r == '0' * (len(key) - 1):
            print("No Error in Transmission")
        else:
            print("Error Detected in Transmission")

# Test example
data = '100100'  # Original message
key = '1101'     # Divisor polynomial (CRC key)

# Create CRC object
c = CRC()

# Sender side encoding
print("Sender Side:")
c.encodedData(data, key)
print('---------------')

# Receiver side decoding
print("Receiver Side:")
c.receiverSide(key, c.cdw)


