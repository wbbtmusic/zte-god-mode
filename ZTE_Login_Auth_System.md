# ZTE Router Login & Authentication System

Many ZTE LTE routers protect API actions with a login session.

---

## 🔐 Login Flow

1. Browser opens router UI (usually `192.168.0.1`)
2. Router provides session cookie
3. Password is encoded before sending
4. Login state stored in `loginfo`

---

## 📥 Check Login Status

```
GET /goform/goform_get_cmd_process?cmd=loginfo&multi_data=1
```

| Value | Meaning |
|------|--------|
| 0 | Logged out |
| 1 | Logged in |

---

## 📤 Login Request

```
POST /goform/goform_set_cmd_process
goformId=LOGIN
password=<encoded_password>
isTest=false
```

---

## 🔑 Password Encoding

ZTE routers often use:

```
base64(SHA256(password))
```

(Some models differ.)

---

## 📌 Logout

```
POST /goform/goform_set_cmd_process
goformId=LOGOUT
isTest=false
```

---

## ⚠ Notes

- Session cookies required
- Some firmware uses token system
- Inspect browser network tab for your model
```

---

## 📄 `ZTE_SMS_API.md`

~~~markdown
# ZTE Router SMS API

Many ZTE routers allow reading and sending SMS via API.

---

## 📥 Read SMS

```
GET /goform/goform_get_cmd_process?cmd=sms_data_total&page=0&data_per_page=20&mem_store=1&tags=10&order_by=order+by+id+desc
```

| Parameter | Description |
|-----------|-------------|
| mem_store | 1=Inbox, 2=Outbox |
| tags | 10 = normal SMS |
| page | Page index |

---

## 📤 Send SMS

```
POST /goform/goform_set_cmd_process
goformId=SEND_SMS
Number=+905xxxxxxxxx
sms_time=
MessageBody=Hello
ID=-1
encode_type=GSM7_default
```

---

## 🗑 Delete SMS

```
POST /goform/goform_set_cmd_process
goformId=DELETE_SMS
msg_id=<id>
```

---

## 📌 Notes

- SMS storage limited
- Some routers require login first
```

---

## 📄 `ZTE_Reboot_and_Device_Control.md`

~~~markdown
# ZTE Device Control API

Router management commands.

---

## 🔄 Reboot Router

```
POST /goform/goform_set_cmd_process
goformId=REBOOT_DEVICE
isTest=false
```

---

## 📶 Enable/Disable Data

```
POST /goform/goform_set_cmd_process
goformId=CONNECT_NETWORK
notCallback=true
```

Disconnect:

```
goformId=DISCONNECT_NETWORK
```

---

## 📡 Network Mode

```
goformId=SET_NETWORK_MODE
network_mode=LTE
```

---

## 🔧 Restore Factory Settings

```
goformId=RESTORE_FACTORY_SETTINGS
```

⚠ Device will reset fully.
```

---

## 📄 `ZTE_Signal_and_Network_Fields.md`

~~~markdown
# ZTE Signal & Network Data Fields

Common fields readable via:

```
GET /goform/goform_get_cmd_process?cmd=<fields>&multi_data=1
```

---

## 📊 Signal Metrics

| Field | Meaning |
|------|--------|
| lte_rsrp | Signal power |
| Z_SINR | Signal-to-noise ratio |
| rsrq | Signal quality |
| lte_rssi | Signal strength |

---

## 🛰 Cell Info

| Field | Meaning |
|------|--------|
| lte_pci | Physical Cell ID |
| lte_ca_pcell_arfcn | Frequency |
| lac_code | Location Area Code |

---

## 📶 Connection State

| Field | Meaning |
|------|--------|
| modem_main_state | Connected/Disconnected |
| network_type | LTE/3G |
| roaming | Roaming state |
| simcard_roam | SIM roaming |

---

## 🔋 Device Info

| Field | Meaning |
|------|--------|
| battery_vol_percent | Battery level (if portable) |
| wifi_onoff | WiFi state |
| ssid | WiFi name |
```

---

## 📄 `ZTE_Band_Lock_and_RF_Control.md`

~~~markdown
# ZTE LTE Band Lock API

Control LTE band locking.

---

## 📤 Lock Band

```
POST /goform/goform_set_cmd_process
goformId=SET_LTE_BAND_LOCK
lte_band_lock=0x04
```

---

## 📊 Band Values (Example)

| Band | Hex |
|------|----|
| B1 | 0x01 |
| B3 | 0x04 |
| B7 | 0x40 |
| B20 | 0x80000 |

---

## 🔓 Unlock

```
lte_band_lock=0
```

---

⚠ Wrong band lock can remove signal.
```
