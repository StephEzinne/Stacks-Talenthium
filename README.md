
#  Stacks-Taalenthium — Decentralized Education Credentials on Stacks

**Stacks-Talenthi** is a decentralized system for managing educational credentials, skills, and peer assessments using the Stacks blockchain and Clarity smart contracts. It allows users to register, submit skill verification requests, and participate as peer assessors. Reputation is used to incentivize honest participation and ensure reliability in credential validation.

---

## ✨ Features

* ✅ **User Registration** — Anyone can register as a verified user.
* 🛠️ **Skill Creation** — Admins can define new skills to be assessed.
* 📋 **Assessment Requests** — Users can request peer assessment for specific skills.
* 📈 **Peer Assessment** — Other users provide scores for assessments.
* 🧮 **Stats-Based Verification** — Assessments use mean and standard deviation to verify authenticity.
* 🌟 **Reputation System** — Encourages fair scoring and penalizes dishonest actors.

---

## 🔧 Smart Contract Details

### 📄 Constants

| Constant                       | Description                                         |
| ------------------------------ | --------------------------------------------------- |
| `contract-owner`               | Initial contract deployer                           |
| `min-assessors`                | Minimum number of assessors per skill (3)           |
| `assessment-threshold`         | Percentage approval required for verification (70%) |
| `max-assessors`                | Max number of assessors allowed (20)                |
| `standard-deviation-threshold` | Max allowed deviation from mean (15%)               |
| `reputation-reward`            | Reputation points for valid assessment (+2)         |
| `reputation-penalty`           | Reputation penalty for invalid assessment (-5)      |

### 🗃 Data Structures

#### `users (principal → user-data)`

Tracks registration, skills, and reputation.

#### `skills (uint → skill-data)`

Defines skill name, description, category, and required assessments.

#### `skill-assessments ({skill-id, user} → assessment-data)`

Stores assessment requests and submissions with score stats.

#### `skill-reputation ({user, skill-id} → rep-data)`

Stores skill-specific reputation for assessors.

---

## 📦 Usage

### 🔐 1. Register as a User

```clarity
(register-user)
```

Registers the transaction sender.

---

### 🧠 2. Add a Skill *(admin only)*

```clarity
(add-skill name description required-assessments category)
```

Admin defines a new skill with metadata.

---

### 🧾 3. Request Assessment

```clarity
(request-assessment skill-id)
```

User requests peer assessment for a skill.

---

### 📊 4. Submit Assessment

```clarity
(submit-assessment skill-id user score)
```

Another user scores the assessment (0–100). Stats like mean and standard deviation are recalculated.

---

## 🧮 Reputation & Validation Logic

* Each assessment affects the assessor's **reputation**.
* If a score deviates more than `15%` from the mean, it's marked **invalid**.
* Users with repeated invalid assessments are **penalized**.
* Accurate assessments result in **reputation rewards**.

---

## 📚 Function Reference

### ✅ `register-user`

Registers a new user if not already registered.

### ✅ `add-skill`

Admin function to define a new skill with validation.

### ✅ `request-assessment`

User requests a new peer review for a specific skill.

### ✅ `submit-assessment`

Allows another user to assess a skill request. Automatically updates:

* Mean score
* Standard deviation
* Assessment list

---

## 🧠 Statistical Helper Functions

* `calculate-mean(scores)`
* `calculate-standard-deviation(scores, mean)`
* `square(x)`
* `square-diff-from-mean(score, mean)`
* `sqrt(x)` *(basic approximation)*

---

## ⚖️ Access Control

* Only `contract-owner` can call `add-skill`.
* Users can’t assess themselves.
* Duplicate assessments are blocked.

---

##  Error Codes

| Code | Description            |
| ---- | ---------------------- |
| 100  | Not authorized         |
| 101  | Already registered     |
| 102  | Not registered         |
| 103  | Insufficient assessors |
| 104  | Already assessed       |
| 105  | Max assessors reached  |
| 106  | Invalid score          |
| 107  | Invalid skill ID       |
| 108  | Invalid input          |

---

## 🚀 Deployment Guide

1. Deploy using Clarity-compatible tool (e.g., Clarinet).
2. Set `tx-sender` as `contract-owner`.
3. Use provided public functions to onboard users and define skills.

---
