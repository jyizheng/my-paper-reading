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

### Focus of LLAMA

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
    - Partial page flushes
    - Low write amplification (Good for flash)
    - No internal fragmentaion


---
