# Contributing to Employee Growth Coach

Thank you for your interest in contributing to the Employee Growth Coach! This document provides guidelines and instructions for contributing.

## 🤝 Code of Conduct

Please note that this project is released with a [Contributor Code of Conduct](./CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

### Our Values
- **Respectful**: Treat all contributors with respect
- **Inclusive**: Welcome contributions from all backgrounds
- **Ethical**: Prioritize privacy and security in all decisions
- **Collaborative**: Work together toward common goals

## 🚀 Getting Started

### Setup Development Environment

```bash
# Clone repository
git clone https://github.com/WMT-Technologies/alweam.git
cd alweam

# Backend setup
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
pip install -r requirements-dev.txt
cp .env.example .env

# Frontend setup
cd ../frontend
npm install
npm install --save-dev

# Root setup
cd ..
```

### Run Tests Locally

```bash
# Backend tests
cd backend
pytest tests/ -v

# Frontend tests
cd frontend
npm test

# Code quality checks
cd backend
flake8 .
black --check .
mypy .

cd ../frontend
npm run lint
```

## 📋 Types of Contributions

### 🐛 Bug Reports
Found a bug? Please create an issue with:
- **Title**: Brief description of the bug
- **Description**: What happened and what you expected
- **Steps to Reproduce**: Detailed steps
- **Environment**: OS, Python/Node version, etc.
- **Screenshots**: If applicable

### ✨ Feature Requests
Have an idea? Create an issue with:
- **Title**: Feature name
- **Motivation**: Why this feature matters
- **Proposed Solution**: How it should work
- **Alternatives**: Other approaches considered
- **Use Cases**: Real-world examples

### 📖 Documentation
Help improve documentation:
- Fix typos or clarify explanations
- Add examples
- Improve diagrams
- Translate to other languages

### 🔧 Code Contributions
Want to contribute code?

1. **Pick an Issue**: Look for issues labeled `good-first-issue` or `help-wanted`
2. **Comment**: Let others know you're working on it
3. **Create Branch**: `git checkout -b feature/your-feature-name`
4. **Make Changes**: Follow coding standards (see below)
5. **Write Tests**: Ensure good coverage
6. **Submit PR**: Create pull request with clear description

## 📝 Development Workflow

### Branch Naming Convention
```
feature/feature-name          # New feature
bugfix/bug-description        # Bug fix
docs/documentation-update     # Documentation
refactor/refactoring-changes  # Code refactoring
test/test-coverage-improvement # Tests
```

### Commit Message Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

Examples:
```
feat(conversation): add sentiment analysis to responses

Implement sentiment detection to ensure supportive tone
in all coach responses.

Fixes #123
```

```
fix(auth): resolve token expiration issue

JWT tokens now properly refresh when nearing expiration.
Prevents unexpected logout during conversations.

Closes #456
```

### Commit Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting, semicolons, etc. (no logic change)
- `refactor`: Code refactoring
- `perf`: Performance improvement
- `test`: Test addition or modification
- `chore`: Dependency update, build config, etc.

## 🎯 Pull Request Process

### Before Submitting
- [ ] Fork the repository
- [ ] Create a feature branch
- [ ] Make changes following coding standards
- [ ] Add/update tests
- [ ] Update documentation
- [ ] Ensure all tests pass locally
- [ ] Rebase on latest main branch
- [ ] Test merge conflicts resolved

### PR Description Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Fixes #123
Related to #456

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing completed

## Screenshots (if UI changes)
[Add screenshots here]

## Checklist
- [ ] Code follows style guidelines
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests added/pass
- [ ] Dependent changes merged
```

### Review Process
1. Automated checks run (linting, tests, security)
2. Code review by maintainers
3. Request changes if needed
4. Approval and merge by maintainer

## 💻 Coding Standards

### Backend (Python)
```python
# Follow PEP 8
# - 4 spaces for indentation
# - Max line length: 88 characters (Black formatter)
# - Use type hints

# Example
from typing import Optional, Dict, List
from pydantic import BaseModel

class ConversationTurn(BaseModel):
    """Represents a single turn in conversation."""
    
    question: str
    answer: str
    timestamp: str
    analysis: Optional[Dict[str, any]] = None
    
    def is_complete(self) -> bool:
        """Check if turn is complete."""
        return bool(self.answer)

async def process_turn(turn: ConversationTurn) -> Dict:
    """Process conversation turn."""
    # Implementation
    pass
```

### Frontend (JavaScript/React)
```javascript
// Use Prettier for formatting
// - 2 spaces for indentation
// - Semicolons required
// - Single quotes for strings

// Use TypeScript for new code
interface ChatMessage {
  id: string;
  content: string;
  sender: 'user' | 'assistant';
  timestamp: Date;
  metadata?: Record<string, unknown>;
}

// Component example
const ChatWindow: React.FC<ChatWindowProps> = ({
  messages,
  onSendMessage,
  isLoading,
}) => {
  return (
    <div className="chat-window">
      {messages.map((msg) => (
        <ChatMessage key={msg.id} message={msg} />
      ))}
    </div>
  );
};

export default ChatWindow;
```

### Testing

#### Backend
```python
import pytest
from unittest.mock import patch, MagicMock

class TestCoachEngine:
    """Test coach engine functionality."""
    
    @pytest.fixture
    def coach_engine(self):
        """Provide coach engine fixture."""
        return CoachEngine()
    
    def test_generate_response_supportive(self, coach_engine):
        """Test that responses are supportive."""
        response = coach_engine.generate_response(
            "I struggle with task X",
            context={"role": "engineer"}
        )
        assert "support" in response.lower() or "help" in response.lower()
    
    def test_no_judgmental_language(self, coach_engine):
        """Ensure no judgmental language in responses."""
        response = coach_engine.generate_response(
            "I made a mistake",
            context={"role": "engineer"}
        )
        judgmental_words = ["failure", "incompetent", "should"]
        for word in judgmental_words:
            assert word not in response.lower()
```

#### Frontend
```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import ChatWindow from './ChatWindow';

describe('ChatWindow', () => {
  it('renders messages correctly', () => {
    const messages = [
      { id: '1', content: 'Hello', sender: 'user' as const },
    ];
    render(<ChatWindow messages={messages} />);
    expect(screen.getByText('Hello')).toBeInTheDocument();
  });

  it('calls onSendMessage when send button clicked', () => {
    const mockSend = jest.fn();
    render(
      <ChatWindow
        messages={[]}
        onSendMessage={mockSend}
        isLoading={false}
      />
    );
    
    fireEvent.click(screen.getByRole('button', { name: /send/i }));
    expect(mockSend).toHaveBeenCalled();
  });
});
```

## 📚 Documentation Standards

### Code Comments
```python
# Good: Explains why, not what
def generate_response(self, input: str) -> str:
    # Check sentiment first to adjust tone
    # We want supportive responses even for negative inputs
    sentiment = self.analyze_sentiment(input)
    ...

# Avoid: Obvious comments
def generate_response(self, input: str) -> str:
    # Generate response
    return self.model.generate(input)
```

### Docstrings
```python
def process_conversation_turn(
    session_id: str,
    user_response: str,
    analysis_context: Optional[Dict] = None,
) -> Dict[str, Any]:
    """
    Process a single conversation turn.
    
    Analyzes the user response in context of the conversation
    and generates appropriate next question or closing.
    
    Args:
        session_id: Unique conversation session identifier
        user_response: The user's response text
        analysis_context: Optional context for analysis
        
    Returns:
        Dictionary containing:
            - next_question: The next question to ask (if continuing)
            - is_complete: Whether conversation is complete
            - analysis: Preliminary analysis results
            
    Raises:
        ValueError: If session_id is invalid
        HTTPException: If external API calls fail
        
    Example:
        >>> result = process_conversation_turn(
        ...     session_id="sess_123",
        ...     user_response="I find X challenging",
        ... )
        >>> print(result["next_question"])
    """
```

## 🔒 Security Considerations

### When Contributing Code
- ❌ Never commit secrets or credentials
- ✅ Use `.env` files for local config
- ✅ Validate all inputs
- ✅ Encrypt sensitive data
- ✅ Follow OWASP guidelines
- ✅ Run security linters

### Reporting Security Issues
Found a security vulnerability?
**Do NOT** create a public issue.

Email: security@wmt-technologies.com

Include:
- Description of vulnerability
- Affected components
- Proof of concept (if possible)
- Suggested fix (if you have one)

## 📊 Performance Guidelines

### Backend
- Database queries should use indexes
- Cache expensive operations
- Limit response sizes
- Set appropriate timeouts
- Monitor query performance

### Frontend
- Lazy load components
- Memoize expensive computations
- Optimize bundle size
- Minimize re-renders
- Use appropriate loading states

## 🧪 Test Coverage

Aim for:
- **Overall**: >80% code coverage
- **Critical Paths**: >95% coverage
- **Utilities**: >90% coverage
- **UI Components**: >70% coverage

Run coverage reports:
```bash
# Backend
cd backend
pytest --cov=. --cov-report=html

# Frontend
cd frontend
npm run test:coverage
```

## 📈 Performance Metrics

When submitting PRs that affect performance:
- Include benchmark results
- Document performance impact
- Test with realistic data volumes
- Consider edge cases

## 🎓 Learning Resources

### Backend
- [FastAPI Docs](https://fastapi.tiangolo.com/)
- [SQLAlchemy Docs](https://docs.sqlalchemy.org/)
- [Pydantic Docs](https://docs.pydantic.dev/)

### Frontend
- [React Docs](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS](https://tailwindcss.com/docs)

### AI/ML
- [OpenAI API Docs](https://platform.openai.com/docs/)
- [Prompt Engineering Guide](https://github.com/brexhq/prompt-engineering)

## 🎉 Recognition

Contributors are recognized in:
- [CONTRIBUTORS.md](./CONTRIBUTORS.md)
- GitHub Contributors page
- Release notes
- Project website

## ❓ Questions?

- 📖 Check documentation in `/docs`
- 💬 Open a discussion on GitHub
- 📧 Email: dev@wmt-technologies.com
- 💭 Join our Slack community (link in repo)

## 📜 License

By contributing, you agree your contributions are licensed under the same license as the project.

---

Thank you for contributing to Employee Growth Coach! 🚀
