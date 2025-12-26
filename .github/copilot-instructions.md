# AI Coding Agent Instructions for 8D Report Assistant

## Project Overview
This is a Streamlit-based 8D problem-solving report management system with AI-powered analysis and auditing capabilities. The app handles quality management workflows for manufacturing and service industries.

## Architecture & Key Components

### Core Structure
- **Single-file app** (`app.py`) - All functionality in one Streamlit application
- **Session state management** - Data persistence across user interactions
- **Two primary workflows**: Report creation (D0-D8) and external report auditing

### Data Flow
1. **Report Creation**: Tabbed interface collects 8D data → AI analysis → HTML/Word/PDF export
2. **Report Auditing**: File upload/text input → AI extraction → structured evaluation → export

## Critical Developer Workflows

### AI Integration Patterns
```python
# Always use this pattern for DeepSeek API calls
client = OpenAI(api_key=api_key, base_url="https://api.deepseek.com")
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[{"role": "user", "content": prompt}],
    temperature=0.2  # Low for structured outputs
)
```

### Session State Management
```python
# Initialize complex nested structures
st.session_state.data = {
    'd0': {'title': '', 'customer': ''},
    'd1': {'leader': '', 'members': ''},
    # ... full 8D structure
}
```

### Export Functionality
- **HTML**: Jinja2 templates with CSS print optimization
- **Word**: python-docx with structured tables
- **PDF**: Browser print with CSS media queries

## Project-Specific Conventions

### AI Prompt Engineering
- Use structured JSON schemas for data extraction
- Include specific field mappings (e.g., D1_TeamLeader, D2_5W2H)
- Separate extraction and evaluation phases
- Preserve `***AI_EVAL_SEP***` delimiter for translation

### UI Patterns
```python
# Section headers with emoji and description
def section(title):
    st.markdown(f"## {title}")
    st.markdown("---")

# Status-based conditional formatting
def get_action_status(action_date_str, current_status):
    # Returns CSS class and display text
```

### Data Structures
- **D3/D5 Actions**: `{"action": str, "owner": str, "dueDate": str, "status": str}`
- **D4 Root Cause**: `{"OccurrenceRootCause": str, "EscapeRootCause": str}`
- **D2 Problem**: 5W2H structure with quantified data

## Integration Points

### External Dependencies
- **DeepSeek API**: Primary AI engine for analysis/translation
- **Supabase**: Optional database for report storage
- **File Processing**: PDF text extraction (pdfminer.six) and plain text

### Cross-Component Communication
- Session state as central data store
- Callback functions for state updates
- Conditional rendering based on session state

## Key Files & Patterns

### Core Logic Locations
- **AI Prompts**: Lines 500-900 in `app.py` - Complex prompt engineering
- **Data Structures**: Lines 300-400 - Session state initialization
- **Export Functions**: Lines 100-300 - Word/HTML generation
- **UI Layout**: Lines 400-500 - Tabbed interface logic

### Common Patterns
- **Error Handling**: Try/except with user-friendly st.error() messages
- **Loading States**: `st.spinner()` for AI operations
- **Conditional UI**: `if st.button()` patterns for dynamic updates
- **Data Validation**: Check for required fields before AI calls

## Development Best Practices

### When Adding Features
1. **Preserve Session State**: Always initialize new keys in session state
2. **Follow AI Patterns**: Use established prompt structures
3. **Maintain Export Compatibility**: Update all export formats (HTML/Word)
4. **Test UI Flow**: Verify tab navigation and conditional displays

### Code Organization
- Keep related functionality together (extraction → evaluation → display)
- Use descriptive variable names matching 8D terminology
- Comment complex AI prompts with expected outputs

### Debugging Tips
- Check session state with `st.write(st.session_state)` during development
- Test AI prompts independently before integration
- Verify export functions with sample data

## Quality Management Context
This app implements the 8D (Eight Disciplines) problem-solving methodology:
- D1-D2: Team formation and problem definition
- D3-D4: Containment and root cause analysis
- D5-D8: Corrective actions, verification, and prevention

AI agents should understand this structured approach when suggesting improvements or analyzing report completeness.