# ContactsManager.io Spam Database

A community-driven repository of spam phone numbers, email addresses, and patterns to improve contact recommendations across the ContactsManager.io ecosystem.

## 🎯 Purpose

This repository serves as a centralized, community-maintained database of:

- Spam and marketing phone numbers
- Spam and promotional email addresses
- Common spam patterns for automated detection
- Domain reputation data

The data can be integrated into any contact management system to filter out unwanted contacts.

## 📁 Repository Structure

```
├── patterns/
│   ├── phone-patterns.json      # Regex patterns for spam phone numbers
│   ├── email-patterns.json      # Regex patterns for spam emails
│   └── domain-patterns.json     # Regex patterns for spam domains
├── blocklists/
│   ├── phone-numbers.json       # Specific spam phone numbers
│   ├── email-addresses.json     # Specific spam email addresses
│   └── domains.json             # Spam domains with reputation scores
├── .github/
│   ├── workflows/
│   │   ├── validate.yml         # PR validation
│   │   └── format-check.yml     # Format validation
│   └── PULL_REQUEST_TEMPLATE.md
├── schemas/
│   ├── phone-pattern.schema.json
│   ├── email-pattern.schema.json
│   ├── phone-blocklist.schema.json
│   ├── email-blocklist.schema.json
│   └── domain-blocklist.schema.json
└── README.md
```

## 📋 Data Formats

### Pattern Files (`patterns/`)

**Phone Patterns (`patterns/phone-patterns.json`)**

```json
{
  "version": "1.0.0",
  "last_updated": "2025-01-XX",
  "patterns": [
    {
      "id": "us-toll-free",
      "regex": "^1?8(00|44|55|66|77|88)[0-9]{7}$",
      "description": "US toll-free numbers",
      "confidence": 1.0,
      "region": "US",
      "category": "toll-free"
    }
  ]
}
```

**Email Patterns (`patterns/email-patterns.json`)**

```json
{
  "version": "1.0.0",
  "last_updated": "2025-01-XX",
  "patterns": [
    {
      "id": "noreply-emails",
      "regex": "^noreply@.*",
      "description": "No-reply email addresses",
      "confidence": 1.0,
      "category": "automated"
    }
  ]
}
```

### Blocklist Files (`blocklists/`)

**Phone Numbers (`blocklists/phone-numbers.json`)**

```json
{
  "version": "1.0.0",
  "last_updated": "2025-01-XX",
  "entries": [
    {
      "number": "18005551234",
      "confidence": 0.95,
      "source": "user_reports",
      "description": "Telemarketing - multiple reports",
      "region": "US",
      "date_added": "2025-01-XX",
      "report_count": 15
    }
  ]
}
```

**Email Addresses (`blocklists/email-addresses.json`)**

```json
{
  "version": "1.0.0",
  "last_updated": "2025-01-XX",
  "entries": [
    {
      "email": "spam@example.com",
      "confidence": 0.98,
      "source": "spam_database",
      "description": "Known spam sender",
      "date_added": "2025-01-XX",
      "report_count": 42
    }
  ]
}
```

**Domains (`blocklists/domains.json`)**

```json
{
  "version": "1.0.0",
  "last_updated": "2025-01-XX",
  "entries": [
    {
      "domain": "spammy-domain.com",
      "confidence": 0.85,
      "source": "domain_reputation",
      "description": "Low reputation domain",
      "date_added": "2025-01-XX",
      "reputation_score": 15
    }
  ]
}
```

## 🤝 Contributing

### Adding Spam Data

1. **Fork this repository**
2. **Choose the appropriate file** based on your data type
3. **Follow the JSON schema** for the file you're editing
4. **Add your entry** with proper confidence scores and descriptions
5. **Submit a Pull Request** with a clear description

### Confidence Scores

- **1.0**: 100% certain (patterns that should always be blocked)
- **0.9-0.99**: Very high confidence (verified spam)
- **0.8-0.89**: High confidence (multiple reports)
- **0.7-0.79**: Medium confidence (some reports)
- **0.6-0.69**: Low confidence (suspicious but uncertain)

### Guidelines

- ✅ **Verified spam only** - Don't add legitimate numbers/emails
- ✅ **Include source** - Specify where the spam report came from
- ✅ **Regional context** - Add region/country when relevant
- ✅ **Clear descriptions** - Explain why this is spam
- ❌ **No personal vendettas** - Don't add personal disputes
- ❌ **No legitimate businesses** - Don't add real companies unless truly spam

## 🔄 Data Updates

This repository is continuously updated by the community:

- **Community contributions**: New spam data added via pull requests
- **Validation**: All entries are validated before acceptance
- **Deduplication**: Duplicate entries are automatically detected
- **Quality control**: Strict guidelines ensure data accuracy

## 📊 Impact

The data from this repository helps organizations worldwide filter spam and unwanted contacts, improving communication quality across various platforms and applications.

## 📝 License

This data is provided under Creative Commons CC0 1.0 Universal - public domain dedication.

## 🚨 Reporting

To report spam numbers or emails:

1. Open an issue with the "spam-report" label
2. Include the number/email and evidence
3. Specify confidence level and source

## 🛠️ Integration

### For Developers

Integrate this spam data into your application:

```bash
# Download latest patterns
curl -s https://raw.githubusercontent.com/contactsmanager-io/spam/main/patterns/phone-patterns.json
curl -s https://raw.githubusercontent.com/contactsmanager-io/spam/main/blocklists/phone-numbers.json

# Or use git submodule for automatic updates
git submodule add https://github.com/contactsmanager-io/spam.git spam-data
```

### Integration Features

- **JSON Schemas**: All data follows strict validation schemas
- **API Access**: Direct access via GitHub's raw content URLs
- **Versioning**: Track data changes through git history
- **Webhooks**: Set up notifications for repository updates
- **Automated Updates**: Use GitHub Actions or cron jobs for syncing

### Example Usage

```javascript
// Example: Load and use phone patterns
const response = await fetch(
  "https://raw.githubusercontent.com/contactsmanager-io/spam/main/patterns/phone-patterns.json"
);
const phonePatterns = await response.json();

function isSpamPhone(phoneNumber) {
  return phonePatterns.patterns.some((pattern) =>
    new RegExp(pattern.regex).test(phoneNumber)
  );
}
```

---

**Made with ❤️ in California**
