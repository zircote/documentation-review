# Documentation Review Criteria

Detailed criteria for evaluating documentation quality.

## Scoring Framework

Rate each criterion on a 1-5 scale:
- **5** - Excellent: Best practice example
- **4** - Good: Minor improvements possible
- **3** - Adequate: Meets basic requirements
- **2** - Below standard: Significant issues
- **1** - Poor: Major problems or missing

## Content Criteria

### Accuracy (Weight: 25%)

| Score | Description |
|-------|-------------|
| 5 | All technical details verified and correct |
| 4 | Minor inaccuracies that don't affect understanding |
| 3 | Some outdated information, examples work |
| 2 | Multiple inaccuracies, some examples fail |
| 1 | Fundamentally incorrect or misleading |

**Check for:**
- Code examples compile/run successfully
- API signatures match implementation
- Version numbers are current
- Configuration options exist
- Stated behaviors are accurate

### Completeness (Weight: 20%)

| Score | Description |
|-------|-------------|
| 5 | Comprehensive coverage, all use cases addressed |
| 4 | Good coverage, edge cases documented |
| 3 | Core functionality documented, gaps exist |
| 2 | Significant features undocumented |
| 1 | Major functionality missing |

**Check for:**
- All features have documentation
- Prerequisites are listed
- Error conditions documented
- Configuration options covered
- Common use cases shown

### Clarity (Weight: 20%)

| Score | Description |
|-------|-------------|
| 5 | Crystal clear, excellent explanations |
| 4 | Clear with minor ambiguities |
| 3 | Generally understandable, some confusion |
| 2 | Frequently unclear or confusing |
| 1 | Incomprehensible or very confusing |

**Check for:**
- Technical terms defined
- Complex concepts explained
- Logical flow of information
- Appropriate detail level
- Clear instructions

### Examples (Weight: 15%)

| Score | Description |
|-------|-------------|
| 5 | Excellent examples, realistic and complete |
| 4 | Good examples, minor improvements possible |
| 3 | Basic examples, could be more helpful |
| 2 | Few or poor examples |
| 1 | No examples or broken examples |

**Check for:**
- Examples are tested and working
- Cover common use cases
- Realistic scenarios
- Progressive complexity
- Expected outputs shown

## Structure Criteria

### Organization (Weight: 10%)

| Score | Description |
|-------|-------------|
| 5 | Perfect logical structure, easy navigation |
| 4 | Well organized, minor improvements possible |
| 3 | Acceptable structure, some navigation issues |
| 2 | Disorganized, difficult to navigate |
| 1 | No coherent structure |

**Check for:**
- Logical section ordering
- Appropriate heading hierarchy
- Related content grouped
- Clear navigation path
- Table of contents (for long docs)

### Formatting (Weight: 5%)

| Score | Description |
|-------|-------------|
| 5 | Perfect formatting, visually excellent |
| 4 | Good formatting, consistent style |
| 3 | Acceptable, minor inconsistencies |
| 2 | Inconsistent or distracting formatting |
| 1 | Poor formatting, hard to read |

**Check for:**
- Consistent markdown syntax
- Code blocks have language tags
- Lists formatted correctly
- Tables render properly
- Whitespace used effectively

### Links (Weight: 5%)

| Score | Description |
|-------|-------------|
| 5 | All links valid, descriptive text |
| 4 | Links work, good descriptions |
| 3 | Most links work, some generic text |
| 2 | Several broken links |
| 1 | Many broken links, unclear destinations |

**Check for:**
- Internal links resolve
- External links accessible
- Link text is descriptive
- Anchor references work
- No orphaned pages

## Issue Categories

### Critical Issues
Must be fixed before publication:
- Incorrect technical information
- Broken code examples
- Security vulnerabilities documented incorrectly
- Missing critical warnings
- Completely missing required sections

### Major Issues
Should be fixed soon:
- Outdated version information
- Missing important use cases
- Unclear procedures
- Broken links to key resources
- Inconsistent terminology

### Minor Issues
Can be addressed over time:
- Typos and grammar errors
- Suboptimal formatting
- Missing edge cases
- Style inconsistencies
- Broken links to optional resources

### Suggestions
Optional improvements:
- Additional examples
- Diagrams for complex concepts
- Cross-references to related docs
- Performance tips
- Alternative approaches

## Review Report Template

```markdown
# Documentation Review: [Document Name]

**Reviewer:** [Name]
**Date:** [Date]
**Document Version:** [Version]

## Summary

[1-2 sentence overall assessment]

## Scores

| Criterion | Score | Weight | Weighted |
|-----------|-------|--------|----------|
| Accuracy | X/5 | 25% | X.XX |
| Completeness | X/5 | 20% | X.XX |
| Clarity | X/5 | 20% | X.XX |
| Examples | X/5 | 15% | X.XX |
| Organization | X/5 | 10% | X.XX |
| Formatting | X/5 | 5% | X.XX |
| Links | X/5 | 5% | X.XX |
| **Total** | | | **X.XX/5** |

## Critical Issues

1. [Issue description]
   - Location: [Section/line]
   - Recommendation: [How to fix]

## Major Issues

1. [Issue description]
   - Location: [Section/line]
   - Recommendation: [How to fix]

## Minor Issues

1. [Issue description]

## Suggestions

1. [Suggestion]

## Conclusion

[Summary and prioritized action items]
```

## Automated Checks

### Markdown Linting
Run markdownlint or similar:
- Heading levels
- List formatting
- Code block syntax
- Line length
- Trailing whitespace

### Link Validation
Check all links:
- Internal file references
- Anchor links
- External URLs
- Image references

### Spelling/Grammar
Run spell checker:
- Technical terms in dictionary
- Consistent spelling
- Grammar issues

### Code Example Testing
For code blocks:
- Syntax validation
- Compilation check
- Runtime testing (where possible)
