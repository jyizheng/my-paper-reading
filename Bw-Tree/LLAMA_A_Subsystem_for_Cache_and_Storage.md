---
marp: true
theme: uncover
paginate: true
---

<style>
.row {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 100%;
}

.column {
  display: flex;
  flex-direction: column;
  flex-basis: 100%;
  flex: 1;
}
</style>

## LLAMA: A Cache/Storage Subsystem for Modern Hardware

---

### Outline

- [Introduction](#2)
- [Design](#3)
- [Implementation](#6)
- [Results](#11)

---

### Introduction

- Designed for new hardware
- Designed for page-oritented access method
- Uses a mapping table for both cache and storage 
    - CL supports data and mgmt updates
        - latch free
    - SL copes with page location changes
        - log structuring on every page flush

---

### Focuses of LLAMA

- A Data Component in Deuteronomy
- Manage storage and retrieval of data accessed
    - CRUD atomic operations
- DC is not distributed but rather is a local mechanism 
    - Can be amalgomated into a distributed system


---

### At Which Layer does LLAMA Work
 ![height:400px](layer.png)

```
LLAMA manages on-disk data and state
```

---

### Mapping Table
 ![height:400px](mt.png)


---
### LLAMA Overview

- Cache
    - Reclaim memory by dropping only flushed portion
    - Control cache size w/o input from accessor
    - Decoupled from TXNs and WAL
- Storage
    - Log-structuring to avoid random writes
    - Partial page flushes (low write amplification)
    - No internal fragmentaion
- System TXNs decoupled from user TXNs

---
### Page Data Operations

- Two types of Operations
    - CRUD
    - Structure changes
- Two updates
    - Delta update
    - Replacement update
- Three APIs
    - Update-D(PID, in-ptr, out-ptr, data)
    - Update-R(PID, in-ptr, out-ptr, data)
    - Read(PID, out-ptr)

--- 
### Page Management Operations

- Manage <span style="color:red;">existence, location and persistence</span> of pages
    - Flush pages to secondary storage
    - Annotate pages for efficiency (LSN)
- Five APIs
    - 