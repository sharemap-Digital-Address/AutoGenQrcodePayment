# AutoGenQrcodePayment
autogen 


phthon

def crc16_ccitt(data: str) -> str:
    crc = 0xFFFF
    for byte in data.encode('utf-8'):
        crc ^= (byte << 8)
        for _ in range(8):
            if crc & 0x8000:
                crc = (crc << 1) ^ 0x1021
            else:
                crc <<= 1
            crc &= 0xFFFF
    return f"{crc:04X}"

# Base layout components without amount
# original length of amount 20.00 was 5 chars -> 540520.00
# for 100.00, length is 6 chars -> 5406100.00

part1 = "00020101021127830012com.p2pqrpay0111GXCHPHM2XXX02089996440303152170200000006560417DWQM4TK3JDNWE4H9052046016"
currency_and_amount = "53036085406100.00"
part2 = "5802PH5910JA****E P.6012Ilayang Iyam610412346304"

full_payload_without_crc = part1 + currency_and_amount + part2
crc_result = crc16_ccitt(full_payload_without_crc)
print(f"Payload: {full_payload_without_crc}{crc_result}")
print(f"CRC: {crc_result}")
