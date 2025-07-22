
# STX-CertiChain - Medical Device Lifecycle & Certification Tracker

`STX-CertiChain` is a Clarity smart contract designed for transparent tracking of medical devices throughout their lifecycle—from manufacturing to deployment—and managing verifiable certifications issued by approved regulatory bodies.

---

## 🧩 Features

* **Device Registration**
  Register new medical devices with initial status and track their ownership.

* **Status Updates**
  Update the status of a device (manufactured, testing, deployed, maintained) while maintaining historical records.

* **Certification Management**
  Only approved regulatory bodies can issue, verify, or revoke certifications (FDA, CE, ISO, etc.).

* **Immutable History**
  Each device maintains a capped historical log (up to 10 entries) of status changes with timestamps.

* **Access Control**
  Admin-restricted operations and authority validation ensure secure usage.

---

## 🧾 Contract Summary

| Element      | Description                                                           |
| ------------ | --------------------------------------------------------------------- |
| **Trait**    | `medical-device-tracking-trait`                                       |
| **Title**    | `STX-CertiChain`                                                              |
| **Language** | Clarity (Stacks Blockchain)                                           |
| **Version**  | *To be defined*                                                       |
| **Purpose**  | Track medical device lifecycle and certification status transparently |

---

## 🔐 Roles & Access

* **Contract Owner:**
  The deploying principal. Can register any device, add regulatory bodies, and revoke certifications.

* **Device Owner:**
  The user who registers a device. Can update the device's status.

* **Regulatory Body:**
  Approved authority that can issue or revoke specific certification types.

---

## ✅ Device Lifecycle Constants

| Status Name  | Constant Name                | Value |
| ------------ | ---------------------------- | ----- |
| Manufactured | `DEVICE_STATUS_MANUFACTURED` | `u1`  |
| Testing      | `DEVICE_STATUS_TESTING`      | `u2`  |
| Deployed     | `DEVICE_STATUS_DEPLOYED`     | `u3`  |
| Maintained   | `DEVICE_STATUS_MAINTAINED`   | `u4`  |

---

## 🧾 Certification Types

| Certification Type | Constant Name      | Value |
| ------------------ | ------------------ | ----- |
| FDA Approval       | `CERT_TYPE_FDA`    | `u1`  |
| CE Certification   | `CERT_TYPE_CE`     | `u2`  |
| ISO Compliance     | `CERT_TYPE_ISO`    | `u3`  |
| Safety Verified    | `CERT_TYPE_SAFETY` | `u4`  |

---

## 📚 Key Functions

### Device Management

```clarity
(register-device (device-id uint) (initial-status uint)) : (response bool uint)
```

Registers a new device with an initial status.

```clarity
(update-device-status (device-id uint) (new-status uint)) : (response bool uint)
```

Updates the status of an existing device and appends to history.

```clarity
(get-device-status (device-id uint)) : (response uint uint)
```

Returns the current status of a device.

```clarity
(get-device-history (device-id uint)) : (response (list 10 {status: uint, timestamp: uint}) uint)
```

Retrieves the status history of a device.

---

### Certification Management

```clarity
(add-certification (device-id uint) (cert-type uint)) : (response bool uint)
```

Adds a certification for a device, if the sender is an approved regulatory body.

```clarity
(verify-certification (device-id uint) (cert-type uint)) : (response bool uint)
```

Checks whether a given certification on a device is valid.

```clarity
(revoke-certification (device-id uint) (cert-type uint)) : (response bool uint)
```

Revokes a certification; allowed only by issuer or contract owner.

```clarity
(get-certification-details (device-id uint) (cert-type uint)) : (response (optional {...}) uint)
```

Retrieves detailed info about a specific certification.

---

### Authority & Admin Functions

```clarity
(add-regulatory-body (authority principal) (cert-type uint)) : (response bool uint)
```

Contract owner can authorize new regulatory bodies to issue certifications.

```clarity
(is-contract-owner (sender principal)) : bool
```

Checks if the sender is the contract owner.

---

## ⚠️ Error Codes

| Error Name                  | Code     | Meaning                    |
| --------------------------- | -------- | -------------------------- |
| `ERR_UNAUTHORIZED`          | `err u1` | Not permitted              |
| `ERR_INVALID_DEVICE`        | `err u2` | Device ID not valid        |
| `ERR_STATUS_UPDATE_FAILED`  | `err u3` | Unable to update           |
| `ERR_INVALID_STATUS`        | `err u4` | Invalid status             |
| `ERR_INVALID_CERTIFICATION` | `err u5` | Certification type invalid |
| `ERR_CERTIFICATION_EXISTS`  | `err u6` | Already certified          |

---

## 🏗️ Data Structures

### `device-details`

Tracks device owner, current status, and history of status updates.

```clarity
{
  device-id: uint,
  owner: principal,
  current-status: uint,
  history: (list 10 {status: uint, timestamp: uint})
}
```

### `device-certifications`

Tracks certification details for each device and cert-type.

```clarity
{
  device-id: uint,
  cert-type: uint,
  issuer: principal,
  timestamp: uint,
  valid: bool
}
```

### `regulatory-bodies`

Stores approved authorities allowed to certify specific cert-types.

```clarity
{
  authority: principal,
  cert-type: uint,
  approved: bool
}
```

---
