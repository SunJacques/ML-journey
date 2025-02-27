---
title: "Flash attention 2"
date: "2025-02-27"
Link: "https://www.youtube.com/watch?v=gBMO1JZav44"
skills: 
---

## Introduction  

The attention mechanism is the bottleneck of the transformer model, making it the most computationally expensive part.  

### Traditional Methods: Approximate Attention  
Common approaches to improving attention efficiency include:  
- **Low-rank approximation**  
- **Sparse attention**  

**Cons:** These methods trade off accuracy for efficiency.  

### Flash Attention: A Better Alternative  
Flash Attention is a **fast, memory-efficient, and exact** method that overcomes these limitations.  

## Memory Types  

| Feature              | HBM (High Bandwidth Memory)         | SRAM (Static RAM)           | DRAM (Dynamic RAM)          |
|----------------------|----------------------------------|----------------------------|----------------------------|
| **Architecture**     | Stacked memory with TSVs, wide interface | Uses flip-flops to store data  | Uses capacitors to store data as electrical charge |
| **Bandwidth**       | Extremely high (up to 256 GB/s or more) | High bandwidth, low latency | Moderate (e.g., 25.6 GB/s) |
| **Power Consumption** | Energy-efficient for high-performance applications | Low per access, higher static power | Higher due to constant refreshing |
| **Use Cases**       | High-end graphics, HPC, AI accelerators | Cache memory, high-speed storage | Main system memory, general computing |
| **Cost**            | Expensive (more than DRAM)         | Most expensive               | Least expensive, cost-effective |
| **Refresh Requirement** | No                              | No                          | Yes (requires periodic refreshing) |
| **Typical Capacity** | High (e.g., 8 GB per stack)       | Low (e.g., 128-256 KB)       | High (e.g., 4-8 GB or more) |
| **Integration**      | Often integrated with GPUs        | Often integrated with CPUs   | Separate modules on the motherboard |
| **Access Speed**    | Fast                               | Very fast                   | Moderate |

## Mechanism
Flash Attention leverages **SRAM speed** while keeping memory usage low.
![Flash attention](images/flashattn_banner.jpg)


### Attention Computation Steps:  
1. **Score Calculation:** $S = QK^T$
2. **Softmax Application:** $A = \text{softmax}(S)$  
3. **Output Computation:** $O = AV$

### How Flash Attention Works  
Instead of computing attention in a memory-heavy way, Flash Attention reduces memory usage through **tiling** and efficient memory access patterns.  

#### Key Optimizations  
- **Safe Softmax:** Prevents numerical overflow by subtracting the maximum value before exponentiation.  
- **Online Softmax:** Computes softmax in a streaming fashion, reducing memory overhead.

It computes the output in a recursive manner, reducing memory usage.

## Conclusion  

Flash Attention is a novel approach to computing attention more efficiently by processing data in a **streaming manner**. It significantly reduces memory requirements while maintaining accuracy.  