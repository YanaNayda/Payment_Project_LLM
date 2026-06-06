# 🏦 Agentic Payment System
### מערכת תשלומים מבוססת סוכנים חכמים

![Python](https://img.shields.io/badge/Python-3.10+-4ecca3?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-4ecca3?style=for-the-badge&logo=jupyter&logoColor=white)
![AI](https://img.shields.io/badge/Multi--Agent-System-3498db?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-2ecc71?style=for-the-badge)

---

## 📋 תיאור הפרויקט

מערכת תשלומים דיגיטלית (בסגנון Bit) המבוססת על ארכיטקטורת סוכנים חכמים (Agentic AI).  
המערכת מדמה העברות כסף, זיהוי הונאות, ביקורת אבטחה ועוד — הכל בתוך Jupyter Notebook.

---

## 🏗️ ארכיטקטורת המערכת

```
משתמש
   ↓
OrchestratorAgent  ← המנהל הראשי
   │
   ├── RouterAgent          → מזהה את כוונת המשתמש
   ├── PaymentSystem        → מבצע פעולות עסקיות
   ├── FraudDetectionAgent  → בודק עסקאות חשודות
   ├── SecurityAgent        → בודק את תקינות המערכת
   ├── CriticAgent          → מעריך את איכות התוצאה
   ├── ExplanationAgent     → מסביר פעולות למשתמש
   ├── ReflectionAgent      → מציע תיקונים לשגיאות
   └── Multi-Agent Debate   → דיון בין סוכנים לפני החלטה
```

---

## 🤖 סוכנים במערכת

### סוכנים חובה

| סוכן | תפקיד | Confidence |
|------|--------|-----------|
| `RouterAgent` | מזהה כוונת משתמש מתוך 14 כוונות אפשריות | 0.82–0.93 |
| `OrchestratorAgent` | מתאם את כל הסוכנים ומנהל את הזרימה | — |
| `FraudDetectionAgent` | מחשב ציון סיכון לפי 4 קריטריונים | דינמי |
| `SecurityAgent` | מבצע 5 בדיקות אבטחה על המערכת | דינמי |
| `ExplanationAgent` | מסביר את הפעולה האחרונה מתוך הזיכרון | 0.90 |
| `CriticAgent` | מעריך תוצאות בסקלה 1–5 ומחליט על fallback | דינמי |

### סוכנים מתקדמים (בחירה)

| סוכן | תפקיד |
|------|--------|
| `ReflectionAgent` | מקבל שגיאה ומציע תיקון מפורט למשתמש |
| `Multi-Agent Debate` | דיון בין FraudAgent ו-SecurityAgent לפני אישור עסקה חריגה |

---

## 💳 פעולות מערכת התשלומים

```python
create_user(name, phone, initial_balance)     # יצירת משתמש וארנק
get_balance(user_id)                          # בדיקת יתרה
transfer_money(sender_id, receiver_id, amount) # העברת כסף
request_payment(requester_id, payer_id, amount) # יצירת בקשת תשלום
approve_payment_request(request_id)           # אישור בקשה
reject_payment_request(request_id)           # דחיית בקשה
get_transactions(user_id)                    # היסטוריית עסקאות
```

### כללים עסקיים
- ❌ לא ניתן להעביר כסף לעצמך
- ❌ לא ניתן ליצור משתמש עם יתרה שלילית
- ❌ לא ניתן לאשר בקשה פעמיים
- ❌ לא ניתן להעביר סכום גבוה מהיתרה הקיימת
- ❌ לא ניתן להעביר סכום שלילי

---

## 🔍 זיהוי הונאות

`FraudDetectionAgent` מחשב `risk_score` לפי 4 קריטריונים:

| קריטריון | ציון סיכון |
|----------|-----------|
| סכום > 80% מהיתרה | +0.4 |
| סכום > 1000₪ | +0.3 |
| יותר מ-3 עסקאות | +0.2 |
| העברות חוזרות לאותו נמען | +0.1 |

```
risk_score < 0.4  → 🟢 LOW RISK
risk_score < 0.7  → 🟡 MEDIUM RISK  → Multi-Agent Debate
risk_score >= 0.7 → 🔴 HIGH RISK    → BLOCKED
```

---

## ⚔️ Multi-Agent Debate

כאשר עסקה חשודה מזוהה, שני סוכנים נכנסים לדיון:

```
FraudDetectionAgent  ←→  SecurityAgent
        ↓                      ↓
     BLOCK/APPROVE          BLOCK/APPROVE
              ↓
        החלטה סופית
        
שניהם מאשרים    → APPROVED ✅
רק Fraud חוסם   → APPROVED WITH WARNING ⚠️ (אם risk < 0.7)
רק Fraud חוסם   → BLOCKED ❌ (אם risk >= 0.7)
שניהם חוסמים   → BLOCKED ❌
```

---

## 🧠 זיכרון המערכת

```python
agent_memory = {
    # מתרגיל 4
    "last_intent":       None,
    "last_tool":         None,
    "last_user_message": None,
    "last_result":       None,
    # חדש
    "last_action":          None,
    "last_user_id":         None,
    "last_transaction":     None,
    "last_payment_request": None,
    "recent_actions":       []   # 5 פעולות אחרונות
}
```

---

## 💾 שמירה וטעינה

המערכת שומרת את כל הנתונים ל-JSON ומטעינה אותם בחזרה:

```python
save_to_json()   # שמירה → payment_system_data.json
load_from_json() # טעינה → שחזור מלא של המצב
```

קובץ JSON כולל: משתמשים, ארנקים, עסקאות, בקשות תשלום, audit log.

---

## 📊 Dashboard

```
╔══════════════════════════════════════════╗
║         💳 PAYMENT SYSTEM DASHBOARD      ║
╠══════════════════════════════════════════╣
║  👥 Users    💰 Balance  📤 Transactions ║
║  🔴 High     🟡 Medium   ⏳ Pending      ║
║  ─────────────────────────────────────  ║
║  USER BALANCES  [████████░░] Alice 800₪  ║
║                 [████░░░░░░] Bob   500₪  ║
║  ─────────────────────────────────────  ║
║  RECENT ACTIONS                          ║
║  → transferMoney by user_1               ║
╚══════════════════════════════════════════╝
```

---

## 🖥️ ממשק משתמש

אפליקציית בנק אינטראקטיבית בתוך Jupyter Notebook:

- 👤 יצירת משתמשים
- 💸 העברת כסף עם בדיקת הונאות אוטומטית
- 💰 בדיקת יתרה
- 📤 בקשות תשלום
- ✅ אישור / ❌ דחיית בקשות
- 📊 Dashboard, 🛡️ Security, 💾 Save JSON

---

## ✅ תוצאות בדיקות

```
============================================================
         FINAL TESTS - מערכת תשלומים
============================================================

📋 TEST 1: Create two users          ✅ PASSED
📋 TEST 2: Valid money transfer       ✅ PASSED
📋 TEST 3: Negative amount           ✅ PASSED
📋 TEST 4: Insufficient funds        ✅ PASSED
📋 TEST 5: Non-existent user         ✅ PASSED
📋 TEST 6: Self transfer             ✅ PASSED
📋 TEST 7: Payment request + approval ✅ PASSED
📋 TEST 8: Double approval           ✅ PASSED

🏆 ALL 8/8 TESTS PASSED!
============================================================
```

---

## 🚀 הפעלה

```bash
# התקנת תלויות
pip install jupyter ipywidgets

# הפעלת המחברת
jupyter notebook final_agentic_payment_project.ipynb
```

**סדר הפעלת תאים:**
1. Cell 1 — Imports
2. Cells 2–11 — Data classes + Payment system
3. Cells 12–22 — Agents
4. Cell 23 — Orchestrator
5. Cell 24+ — Tests + Dashboard + UI

---

## 📁 מבנה הקובץ

```
final_agentic_payment_project.ipynb
├── Cell 1      # Imports
├── Cells 2-7   # AgentResult + Data classes + DB
├── Cell 8      # print_result (UI helper)
├── Cells 9-15  # Payment system functions
├── Cells 16-17 # Ollama + RouterAgent
├── Cells 18-20 # SentimentAgent + select_tool
├── Cells 21-22 # agent_memory + update_memory
├── Cell 23     # OrchestratorAgent
├── Cell 24     # CriticAgent
├── Cell 25     # ExplanationAgent
├── Cells 26-27 # SecurityAgent
├── Cells 28-29 # FraudDetectionAgent
├── Cells 30-31 # ReflectionAgent
├── Cells 32-33 # Multi-Agent Debate
├── Cells 34-36 # JSON save/load
├── Cell 37     # Orchestrator tests
├── Cell 38     # Final 8 tests ✅
├── Cell 39     # Dashboard 📊
└── Cell 40     # Bank App UI 🏦
```

---

## 👩‍💻 פרטי הפרויקט

| | |
|---|---|
| **קורס** | Agentic AI |
| **סוג** | פרויקט גמר |
| **שפה** | Python 3.10+ |
| **סביבה** | Jupyter Notebook |
| **מודל** | Ollama (llama3) — אופציונלי |

---

*Built with 🤖 Multi-Agent Architecture*
