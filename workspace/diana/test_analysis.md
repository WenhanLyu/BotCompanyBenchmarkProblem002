# ACMOJ Test Data Analysis - int2048 (Problems 2014-2019)

**Analysis Date**: 2026-02-20
**Analyst**: Diana
**Repository**: tbc-pdb-002

## Executive Summary

Analyzed 6 ACMOJ problems (2014-2019) testing big integer arithmetic implementation with **exponentially increasing complexity**. Tests progress from basic 10^1000 operations to extreme 10^500000 stress tests requiring advanced algorithms (Karatsuba/FFT multiplication, Newton division).

**Critical Finding**: Tests 2017-2019 are **performance-critical** and require optimized algorithms to pass within time limits.

---

## Test Structure Overview

### Test Categories
- **Corner**: 7 test generators - Edge cases and correctness validation
- **Integer1**: 5 test generators - Basic operations (2014: Basic Test)
- **Integer2**: 18 test generators - Operator overloading (2015-2019)

### File Structure
- `.cpp` files: Test generators using int2048.h
- `.in` files: Pre-generated input data for stress tests
- Largest input: Integer2/12.in (10 MB)

---

## Problem Progression Analysis

### 2014 - Basic Test (Integer1)
**Operations**: read, print, add, minus
**Constraints**: Numbers ≤ 10^1000
**Time Limit**: 1000 ms
**Memory Limit**: 47 MiB

**Test Patterns**:
- **Test 1**: Constructor validation (default, long long, string, copy)
- **Test 1**: Zero handling ("-0" normalization)
- **Test 5**: 1000 alternating add/minus operations with ~1000-digit numbers

**Edge Cases**:
- Negative zero normalization required
- Leading zeros in input ("0000000000001145141919810")
- Very large numbers (1000+ digits)

**Validation Strategy**: String-based I/O, no optimization needed

---

### 2015 - Operator Overloading Test
**Operations**: All operators (+=, -=, ==, <<, >>, etc.)
**Constraints**: Numbers ≤ 10^1000
**Time Limit**: 1000-5000 ms
**Memory Limit**: 47 MiB

**Test Patterns**:
- **Test 1**: Same as Integer1/1 but using operator= instead of constructor
- **Tests 2-5**: Large number operations (800KB-1MB input files)

**Critical Requirements**:
- Assignment operator must work correctly
- Stream operators (>> and <<) must handle large numbers
- Comparison operators must handle all sign combinations

---

### 2016 - Digit Compression Test
**Operations**: All operations, emphasizing base conversion efficiency
**Constraints**:
- Basic tests: ≤ 10^1000
- Compression tests: ≤ 10^30000 (division operands ≤ 10^3000)
**Time Limit**: 1000-2500 ms
**Memory Limit**: 47 MiB

**Test Patterns**:
- **Test 12**: Fibonacci-like growth (100 iterations × 4000 additions)
  - Generates numbers up to ~10^30000
  - Tests base-10000 or base-100000000 compression

**Critical Requirements**:
- Base conversion (store digits in base 10^4 or 10^8, not base 10)
- Efficient large number handling
- Division must work with 3000-digit divisors

**Validation Strategy**:
- Numbers grow exponentially: a[i] ≈ Fibonacci(4000×i)
- Final numbers have ~30,000 digits

---

### 2017 - Multiplication Speed Test
**Operations**: Multiplication performance
**Constraints**: Numbers ≤ 10^200000
**Time Limit**: 1000 ms
**Memory Limit**: 47 MiB

**Test Patterns**:
- **Test 15**: 100 multiplications of 5000-digit × 5000-digit
  - Standard time: 0.05s (reference solution)
- **Test 17**: 10 multiplications of 500,000-digit × 500,000-digit
  - Standard time: 0.58s (reference solution)
  - **CRITICAL**: Requires Karatsuba or FFT multiplication

**Critical Requirements**:
- **O(n log n)** or **O(n^1.58)** multiplication algorithm required
- Naive O(n^2) will **timeout** on test 17
- Must complete 10 × (500k × 500k) in under 1000ms

**Algorithm Requirements**:
- Basic implementation: Good enough for test 15
- Advanced implementation: Karatsuba (O(n^1.58)) or FFT (O(n log n)) for test 17

---

### 2018 - Division Speed Test
**Operations**: Division performance
**Constraints**: Numbers ≤ 10^12000
**Time Limit**: 1000 ms
**Memory Limit**: 47 MiB

**Test Patterns**:
- **Test 14**: 100 divisions of 100-digit ÷ 50-digit
  - Standard time: 0.01s
- **Test 16**: 100 divisions of 5000-digit ÷ 2500-digit
  - Standard time: 0.09s
- **Test 18**: 10 divisions of 500,000-digit ÷ 250,000-digit
  - Standard time: 1.12s (reference solution)
  - **CRITICAL**: Requires Newton-Raphson or optimized division

**Critical Requirements**:
- **O(n log n)** or better division algorithm
- Naive long division O(n^2) will **timeout** on test 18
- Must complete 10 × (500k ÷ 250k) in under 1000ms

**Algorithm Requirements**:
- Basic: Long division good for tests 14-16
- Advanced: Newton-Raphson iteration (reuses fast multiplication) for test 18

---

### 2019 - Stress Test
**Operations**: Comprehensive stress test of all operations
**Constraints**: Numbers ≤ 10^500000
**Time Limit**: 1000-10000 ms (variable per test case)
**Memory Limit**: 190 MiB (increased from 47 MiB)

**Test Patterns**:
- Combines all previous tests
- Includes both test 17 and test 18 patterns
- May include mixed operations on extremely large numbers

**Critical Requirements**:
- All optimizations from 2017 and 2018 required
- Memory efficiency crucial (190 MiB limit)
- Must handle 500,000-digit numbers across all operations

---

## Edge Cases Identified

### Corner Test Cases (7 tests)

#### Corner/1: Division/Modulo Sign Handling
```cpp
// Tests division floor behavior (Python-style)
x = "1145141919810", y = "-1145141919809"
x / y, -x / y, x / -y, -x / -y
x % y, -x % y, x % -y, -x % -y
```
**Critical**: Division rounds toward negative infinity, not toward zero!

#### Corner/2: Division by Small Numbers
```cpp
// Tests 0/x, 1/x, -1/x for both positive and negative x
x = "-1919810", then x = "1919810"
0 / x, 1 / x, -1 / x
0 % x, 1 % x, -1 % x
```

#### Corner/5: Operator and Constructor Edge Cases
```cpp
// Tests multiple edge cases in one test
- Constructor from string starting with '-' (just "-")
- Unary operators (+a, -a)
- Operators with mixed integer types (int16_t, uint16_t, long long, unsigned)
- Operations with 0 and 1
- Numbers near power-of-10 boundaries (-999...999 + 1)
```

#### Other Corner Cases (3, 4, 6, 7)
- Test 3: Likely comparison operators with edge values
- Test 4: Likely modulo operation correctness
- Test 6: Likely compound operators
- Test 7: Likely mixed operations

---

## Performance Requirements

### Time Complexity by Problem

| Problem | Operation | Naive (O(n²)) | Required | Algorithm |
|---------|-----------|---------------|----------|-----------|
| 2014 | Add/Subtract | ✓ Pass | O(n) | Schoolbook |
| 2015 | All basic | ✓ Pass | O(n) | Schoolbook |
| 2016 | All (30k digits) | ⚠ Marginal | O(n²) or better | Base compression |
| 2017 | Multiply (500k) | ✗ **FAIL** | O(n log n) | Karatsuba/FFT |
| 2018 | Divide (500k) | ✗ **FAIL** | O(n log n) | Newton-Raphson |
| 2019 | All (500k) | ✗ **FAIL** | O(n log n) | Karatsuba + Newton |

### Reference Solution Timing
- Test 17 (500k×500k multiply): **0.58s** standard
- Test 18 (500k÷250k divide): **1.12s** standard

**Implication**: With -O2 optimization and 1000ms limit, there's ~40-50% time margin.

---

## Critical Edge Cases for Implementation

### 1. Division Floor Behavior
**Problem**: C++ division rounds toward zero, requirement is floor division (toward -∞)
```
C++:     10 / -3 = -3,  -10 / 3 = -3
Python:  10 // -3 = -4, -10 // 3 = -4
```
**Solution**: Implement Python-style floor division explicitly

### 2. Modulo Definition
**Formula**: `x % y = x - (x / y) * y`
**Implication**: Modulo sign follows divisor sign (Python-style)

### 3. Zero Normalization
- "0" and "-0" must both print as "0"
- Leading zeros must be handled: "0000123" → "123"
- Result of 0 - 0 should be "0", not "-0"

### 4. Constructor Edge Cases
- String constructor with just "-" (error or zero?)
- Empty string handling
- Very long strings (500,000 characters)

### 5. Overflow in Intermediate Operations
- Addition of two 500k-digit numbers needs 500k+1 digit result
- Multiplication of two 500k-digit numbers needs 1M-digit result
- Memory allocation must handle this (190 MiB limit)

---

## Validation Strategy Recommendations

### Local Testing Approach
1. **Compile test generators**: g++ -std=c++20 -O2 test.cpp int2048.cpp -o test
2. **Generate outputs**: Run tests and compare with reference implementation
3. **Performance testing**: Measure actual runtime on tests 17-18

### Critical Test Points
- **Correctness**: All Corner tests must pass (sign handling, edge cases)
- **Basic performance**: Tests 12-14 (intermediate size)
- **Advanced performance**: Tests 15-16 (5k digits)
- **Extreme performance**: Tests 17-18 (500k digits)

### Progressive Development Strategy
1. **Phase 1**: Basic implementation (pass 2014)
   - Schoolbook addition/subtraction
   - String I/O
   - Pass all Corner tests

2. **Phase 2**: Operator overloading (pass 2015)
   - All operators
   - Comparison operators
   - Stream operators

3. **Phase 3**: Digit compression (pass 2016)
   - Base 10^8 storage
   - Multiplication (naive O(n²))
   - Division (long division O(n²))

4. **Phase 4**: Fast multiplication (pass 2017)
   - Karatsuba multiplication O(n^1.58)
   - OR FFT multiplication O(n log n)

5. **Phase 5**: Fast division (pass 2018)
   - Newton-Raphson division O(n log n)
   - Requires fast multiplication

6. **Phase 6**: Stress test (pass 2019)
   - Memory optimization
   - Combined operations
   - Edge case handling

---

## Memory Considerations

### Memory Limits
- Problems 2014-2018: **47 MiB**
- Problem 2019: **190 MiB**

### Memory Usage Estimation
**Base-10 storage**: 500,000 digits × 1 byte = 500 KB per number
**Base-10^8 storage**: 500,000 digits ÷ 8 × 4 bytes = 250 KB per number

**Multiplication result**: 1,000,000 digits ÷ 8 × 4 bytes = 500 KB

**Total worst case** (2019):
- Input numbers: 2 × 250 KB = 500 KB
- Temporary during operation: ~2 MB (intermediate results)
- Output: 500 KB
- **Total**: < 10 MB (well within 190 MiB limit)

**Conclusion**: Memory is not a bottleneck with proper base compression.

---

## Test Data Size Summary

| Category | Tests | Input Files | Max File Size | Number Range |
|----------|-------|-------------|---------------|--------------|
| Corner | 7 | 0 (self-contained) | - | Edge cases |
| Integer1 | 5 | 4 (.in files) | 1 MB | 10^1000 |
| Integer2 | 18 | 9 (.in files) | 10 MB | 10^500000 |

**Key Observations**:
- Only some tests have pre-generated .in files
- Tests without .in files generate data internally
- Largest .in file is test 12 (Fibonacci growth)

---

## Critical Success Factors

### Must-Have for Passing All Tests
1. ✓ **Correct division floor behavior** (Python-style)
2. ✓ **Correct modulo definition** (x - (x/y)*y)
3. ✓ **Zero normalization** (no "-0")
4. ✓ **Base compression** (base 10^8 for efficiency)
5. ✓ **Fast multiplication** (Karatsuba or FFT for 2017)
6. ✓ **Fast division** (Newton-Raphson for 2018)
7. ✓ **All operator overloads** working correctly
8. ✓ **Sign handling** in all operations

### Nice-to-Have for Development
- Move constructors/assignment (C++11 optimization)
- swap() implementation for efficiency
- Reserve() for vector growth (reduce allocations)

---

## Recommended Testing Workflow

### Pre-Submission Checklist
1. All Corner tests pass (correctness validation)
2. Integer1 tests pass (basic operations)
3. Integer2 tests 1-11 pass (operators, basic performance)
4. Test 12 passes within time (digit compression works)
5. Tests 13-14 pass (multiplication/division basic)
6. Tests 15-16 pass (5k-digit operations)
7. **Test 17 passes** (500k multiplication) ← Critical for 2017
8. **Test 18 passes** (500k division) ← Critical for 2018
9. All tests combined work (2019 stress test)

### Debugging Strategy
- If Corner tests fail: Fix correctness issues first
- If test 12 fails: Implement base compression
- If test 17 fails: Implement Karatsuba/FFT
- If test 18 fails: Implement Newton-Raphson
- If memory limit exceeded: Check base compression and temporary allocations

---

## Next Steps for Implementation Team

1. **Marcus (Architecture)**: Design class structure with base-10^8 storage
2. **Clara (Algorithms)**: Research and implement Karatsuba + Newton-Raphson
3. **Implementation**: Start with Phase 1, validate with Corner tests
4. **Testing**: Generate expected outputs from test generators for validation

---

**End of Analysis**
