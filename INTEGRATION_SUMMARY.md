# Spam Data Repository Integration Guide

## Overview

This document provides guidance for integrating the community spam repository (https://github.com/contactsmanager-io/spam) into contact management systems and applications.

## Integration Architecture

### 1. Repository Structure

- **Patterns**: Regex-based filters for automatic spam detection
- **Blocklists**: Specific numbers/emails/domains with confidence scores
- **Schemas**: JSON validation schemas ensuring data quality
- **Workflows**: GitHub Actions for automated validation

### 2. Application Integration

- **API Access**: Direct HTTP access to raw JSON files via GitHub
- **Local Sync**: Download and cache data locally for offline use
- **Real-time Updates**: Use GitHub webhooks for immediate notifications
- **Version Control**: Track data changes through git commits

### 3. Filtering Implementation

- **Pattern Matching**: Use regex patterns for automatic spam detection
- **Confidence Scoring**: Implement thresholds based on confidence levels
- **Two-Layer Approach**: Combine pattern-based and blocklist-based filtering

## Integration Methods

### Direct API Access

```bash
# Download latest data
curl -s https://raw.githubusercontent.com/contactsmanager-io/spam/main/patterns/phone-patterns.json
curl -s https://raw.githubusercontent.com/contactsmanager-io/spam/main/blocklists/phone-numbers.json
```

### Git Submodule

```bash
# Add as submodule for automatic updates
git submodule add https://github.com/contactsmanager-io/spam.git spam-data
git submodule update --remote
```

### Automated Sync

```javascript
// Example: Automated data fetching
async function updateSpamData() {
  const patterns = await fetch(
    "https://raw.githubusercontent.com/contactsmanager-io/spam/main/patterns/phone-patterns.json"
  ).then((r) => r.json());

  const blocklist = await fetch(
    "https://raw.githubusercontent.com/contactsmanager-io/spam/main/blocklists/phone-numbers.json"
  ).then((r) => r.json());

  return { patterns, blocklist };
}
```

## Implementation Examples

### Pattern-Based Filtering

```javascript
function isSpamByPattern(phoneNumber, patterns) {
  return patterns.some((pattern) => {
    const regex = new RegExp(pattern.regex);
    return regex.test(phoneNumber) && pattern.confidence >= 0.8;
  });
}
```

### Blocklist Filtering

```javascript
function isSpamByBlocklist(phoneNumber, blocklist) {
  const entry = blocklist.find((item) => item.number === phoneNumber);
  return entry && entry.confidence >= 0.8;
}
```

### Combined Filtering

```javascript
function isSpamContact(phoneNumber, emailAddress, spamData) {
  // Check patterns (100% confidence)
  if (isSpamByPattern(phoneNumber, spamData.phonePatterns)) return true;
  if (isSpamByPattern(emailAddress, spamData.emailPatterns)) return true;

  // Check blocklists (configurable confidence)
  if (isSpamByBlocklist(phoneNumber, spamData.phoneBlocklist)) return true;
  if (isSpamByBlocklist(emailAddress, spamData.emailBlocklist)) return true;

  return false;
}
```

## Data Quality Assurance

### Automatic Validation

- JSON schema validation on every PR
- Regex pattern testing to ensure they compile
- Confidence score validation (0.0-1.0 range)
- Duplicate detection and prevention

### Community Moderation

- Pull request templates guide contributors
- Clear guidelines for legitimate vs. spam data
- Source attribution requirements
- Regional and category tagging

## Benefits

### For Organizations

- Reduced spam and unwanted contact exposure
- Community-driven, always up-to-date spam database
- Configurable sensitivity levels
- Open source and transparent

### For Developers

- Easy integration with existing systems
- Extensible pattern and blocklist system
- No licensing costs or restrictions
- Active community maintenance

## Best Practices

### Implementation

1. **Caching**: Cache data locally to reduce API calls
2. **Fallbacks**: Handle network failures gracefully
3. **Performance**: Index data for fast lookups
4. **Updates**: Implement regular data refresh cycles

### Data Handling

1. **Privacy**: Hash sensitive data when storing locally
2. **Validation**: Validate data integrity after downloads
3. **Versioning**: Track data version for debugging
4. **Monitoring**: Log filtering statistics for analysis

## Contributing Back

### Reporting New Spam

1. Fork the repository
2. Add spam data to appropriate JSON files
3. Follow the contribution guidelines
4. Submit a pull request with evidence

### Pattern Development

1. Test patterns thoroughly before submission
2. Provide clear descriptions and categorization
3. Include confidence scores based on certainty
4. Consider regional variations

## Maintenance

### Regular Tasks

- **Data Updates**: Check for repository updates regularly
- **Pattern Review**: Evaluate pattern effectiveness
- **Community Engagement**: Participate in discussions and improvements

### Monitoring

- **False Positives**: Track and report legitimate contacts being filtered
- **Coverage**: Monitor spam detection rates
- **Performance**: Measure filtering speed and accuracy

---

**Note**: This repository provides a scalable, community-driven solution for spam detection that can be integrated into any contact management system or communication platform.
