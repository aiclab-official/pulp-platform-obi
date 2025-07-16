# OBI Type Definition Macros (`typedef.svh`)

This file provides a set of SystemVerilog macros to automatically generate all the required data types (typedefs) for an OBI (Open Bus Interface) bus instance, based on a configuration struct.

## 1. Header Guards

The file starts with header guards to prevent multiple inclusion:

```systemverilog
`ifndef OBI_TYPEDEF_SVH
`define OBI_TYPEDEF_SVH
...
`endif // OBI_TYPEDEF_SVH
```

## 2. Channel and Optional Field Typedef Macros

The file defines macros to generate `typedef struct` types for OBI bus channels and their optional fields:

- **A Channel (Address/Write):**
  - `OBI_TYPEDEF_A_CHAN_T` and `OBI_TYPEDEF_TYPE_A_CHAN_T` generate the struct for the address/write channel.
- **R Channel (Read):**
  - `OBI_TYPEDEF_R_CHAN_T` and `OBI_TYPEDEF_TYPE_R_CHAN_T` generate the struct for the read channel.
- **Optional Fields:**
  - Macros like `OBI_TYPEDEF_MINIMAL_A_OPTIONAL`, `OBI_TYPEDEF_ATOP_A_OPTIONAL`, and `OBI_TYPEDEF_ALL_A_OPTIONAL` generate the optional fields for the A channel, depending on the configuration.
  - Similarly, `OBI_TYPEDEF_MINIMAL_R_OPTIONAL` and `OBI_TYPEDEF_ALL_R_OPTIONAL` handle the R channel optional fields.

## 3. Request and Response Typedef Macros

Macros to generate the request and response structs:
- **Request:**
  - `OBI_TYPEDEF_DEFAULT_REQ_T`, `OBI_TYPEDEF_REQ_T`, and `OBI_TYPEDEF_INTEGRITY_REQ_T` generate different versions of the request struct, depending on the bus features.
- **Response:**
  - `OBI_TYPEDEF_RSP_T` and `OBI_TYPEDEF_INTEGRITY_RSP_T` generate the response struct.

## 4. Main Macro: `OBI_TYPEDEF_ALL`
This is the main macro you will use. It generates all the necessary typedefs for a full OBI bus instance:
```systemverilog
`OBI_TYPEDEF_ALL(obi, ObiCfg)
```
This expands to:
- `obi_a_optional_t` (optional fields for A channel)
- `obi_a_chan_t` (A channel struct)
- `obi_req_t` (request struct)
- `obi_r_optional_t` (optional fields for R channel)
- `obi_r_chan_t` (R channel struct)
- `obi_rsp_t` (response struct)

All these types are generated with the correct widths and fields, based on the configuration struct `ObiCfg`.

### How does it work?
- The macro uses the **token pasting operator** (``) to concatenate the prefix (e.g., `obi`) with the type name, creating unique type names for each bus instance.
- The macro parameters (like `cfg.AddrWidth`, `cfg.DataWidth`, etc.) are taken from your configuration struct.

## 5. Minimal Macro: `OBI_TYPEDEF_DEFAULT_ALL`
If you want a minimal set of types (without all optional fields), use:
```systemverilog
`OBI_TYPEDEF_DEFAULT_ALL(obi, ObiCfg)
```
This generates only the minimal required types for a basic OBI bus.

## 6. Why Use These Macros?
- **No manual typedefs:** You don't have to write out all the structs for every bus instance.
- **Configurable:** If you change your config (e.g., data width, optional fields), the types update automatically.
- **Consistent:** All modules using the same config will have matching types.

## 7. Example Usage
```systemverilog
`include "obi/typedef.svh"

module my_obi_module #(parameter obi_pkg::obi_cfg_t ObiCfg = obi_pkg::ObiDefaultConfig) ( ... );
  `OBI_TYPEDEF_ALL(obi, ObiCfg)
  // Now you can use obi_req_t, obi_rsp_t, etc.
endmodule
```

## 8. Summary Table
| Macro                        | What it does                                    |
|------------------------------|-------------------------------------------------|
| `OBI_TYPEDEF_A_CHAN_T`       | Typedef for address/write channel               |
| `OBI_TYPEDEF_R_CHAN_T`       | Typedef for read channel                        |
| `OBI_TYPEDEF_REQ_T`          | Typedef for request struct                      |
| `OBI_TYPEDEF_RSP_T`          | Typedef for response struct                     |
| `OBI_TYPEDEF_ALL`            | Calls all the above for a full OBI bus instance |

---

**In short:**
- Use `OBI_TYPEDEF_ALL(obi, ObiCfg)` to generate all OBI bus types for your configuration.
- The macros ensure your types are always correct and up-to-date with your config.
