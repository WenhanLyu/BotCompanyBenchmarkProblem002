# Project Roadmap: int2048 Big Integer Arithmetic

**Project Goal**: Implement a high-quality int2048 class that passes all 6 ACMOJ problems (2014-2019)

**Status**: Evaluation complete - Starting M1 implementation

---

## Milestone Plan

### M1: Foundation with Correct Floor Division ✓ Defined
**Goal**: Implement basic int2048 class passing problems 2014 & 2015
**Estimated cycles**: 8
**Status**: Ready for Ares

**Deliverables:**
- int2048.h and int2048.cpp with all required interfaces
- Correct floor division semantics (Python-style, NOT C++ truncate-to-zero)
- All basic operations and operator overloading
- Local testing validates against 2014/2015 test data
- Clean, reviewable code structure

**Critical success factors:**
- Floor division: -10/3 = -4, 10/-3 = -4 (not -3)
- All sign combinations tested
- Zero normalization (-0 → 0)

### M2: Base Compression & Optimization (Planned)
**Goal**: Add digit compression, pass problem 2016
**Target**: Numbers up to 10^30000

### M3: Fast Multiplication (Planned)
**Goal**: Implement FFT/NTT multiplication, pass problem 2017
**Target**: Numbers up to 10^200000

### M4: Fast Division (Planned)
**Goal**: Optimize division algorithm, pass problem 2018
**Target**: Numbers up to 10^12000

### M5: Stress Test & Final Polish (Planned)
**Goal**: Pass problem 2019, all optimizations
**Target**: Numbers up to 10^500000

---

## Evaluation Summary (Completed 2026-02-20)

**Team Assessment:**
- Clara: Requirements analysis - identified floor division as #1 risk
- Marcus: Algorithm research - FFT/NTT mandatory for large-scale problems
- Diana: Test data analysis - validated performance requirements

**Key Findings:**
- Floor division semantics differ from C++ standard (highest risk)
- Simple O(n²) algorithms will fail on problems 2017-2019
- Base 10^8 or 10^9 storage essential for performance
- 12 submission budget requires high-quality local testing

**Implementation Strategy:**
1. M1-M2: Establish correctness foundation (problems 2014-2016)
2. M3-M4: Add advanced algorithms (problems 2017-2018)
3. M5: Final optimization and stress testing (problem 2019)

---

## History

**2026-02-20**: Project initiated, evaluation completed, M1 defined
