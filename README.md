# 🚀 RSpec Testing Agents for Rails

A collection of specialized AI agents designed to write comprehensive RSpec test suites for Ruby on Rails applications. Each agent focuses on specific testing scenarios, ensuring best practices and consistent test coverage across your Rails codebase.

## 🎯 What Are These Agents?

These are specialized prompts/instructions that transform your AI coding assistant (Claude Code, Cursor, GitHub Copilot, etc.) into expert RSpec test writers. Each agent has deep knowledge of specific Rails testing patterns and will help you write better tests faster.

## 📦 Installation & Setup

### For Claude Code (Anthropic)

Claude Code supports two configuration approaches:

#### Option 1: Global Configuration (Apply to All Projects)

1. Clone this repository to a location you'll keep permanently:
```bash
git clone https://github.com/yourusername/rspec-agents.git ~/rspec-agents
```

2. Add agent references to your global Claude configuration file `~/.claude/CLAUDE.md`:
```bash
# Edit your global CLAUDE.md file
echo "# RSpec Testing Agents" >> ~/.claude/CLAUDE.md
echo "When working with RSpec tests, use the agents from ~/rspec-agents/" >> ~/.claude/CLAUDE.md
echo "Start with ~/rspec-agents/rspec-agent.md as the orchestrator" >> ~/.claude/CLAUDE.md
```

3. Now the agents are available in all your projects. Use them with:
```
Use the Task tool to launch the RSpec Orchestrator agent from ~/rspec-agents/rspec-agent.md
```

#### Option 2: Project-Specific Configuration

1. Clone the agents into your Rails project:
```bash
cd /path/to/your/rails/project
git clone https://github.com/yourusername/rspec-agents.git .claude-agents
```

2. Create or update your project's `CLAUDE.md` file:
```bash
cat >> CLAUDE.md << 'EOF'

## RSpec Testing Agents

This project includes specialized RSpec testing agents in .claude-agents/
- Use .claude-agents/rspec-agent.md as the main orchestrator
- Each agent handles specific Rails testing scenarios
- Always start with the orchestrator for complex testing needs
EOF
```

3. Reference agents in your prompts:
```
Use the Task tool with the RSpec agent from .claude-agents/rspec-agent.md
```

### For Cursor

1. Add agent files to your `.cursorrules` directory:
```bash
mkdir -p .cursor/agents
cp rspec-*.md /path/to/project/.cursor/agents/
```

2. Reference in your cursor instructions or directly in prompts

### For GitHub Copilot

1. Add to `.github/copilot-instructions.md`:
```bash
cat rspec-agent.md >> .github/copilot-instructions.md
```

## 🤖 Available Agents

### 🎭 Master Orchestrator
- **[rspec-agent.md](rspec-agent.md)** - The main coordinator that analyzes your testing needs and delegates to specialized agents

### 🧩 Core Testing Agents

| Agent | Purpose | Use When |
|-------|---------|----------|
| **[Model Specs](rspec-model-specs-agent.md)** | ActiveRecord models, validations, scopes | Testing business logic, data integrity |
| **[Request Specs](rspec-request-specs-agent.md)** | API endpoints, controllers, HTTP | Testing REST APIs, authentication |
| **[System Specs](rspec-system-specs-agent.md)** | Full UI workflows, JavaScript | End-to-end user journeys |

### 🛠️ Rails Component Agents

| Agent | Purpose | Use When |
|-------|---------|----------|
| **[ActiveJob](rspec-active-job-agent.md)** | Background jobs, async processing | Testing Sidekiq, DelayedJob workers |
| **[ActionMailer](rspec-action-mailer-agent.md)** | Email functionality | Testing email content, delivery |
| **[ActiveStorage](rspec-active-storage-agent.md)** | File uploads, attachments | Testing file handling, image processing |
| **[ActionCable](rspec-actioncable-agent.md)** | WebSockets, real-time features | Testing chat, notifications, live updates |

### 🎯 Testing Strategy Agents

| Agent | Purpose | Use When |
|-------|---------|----------|
| **[TDD Advice](rspec-tdd-advice-agent.md)** | Test-first development coaching | Starting new features, learning TDD |
| **[Isolation Testing](rspec-isolation-testing-agent.md)** | Mocks, stubs, external services | Testing payment gateways, APIs |
| **[DRY Principles](rspec-dry-agent.md)** | Refactoring, reducing duplication | Cleaning up test suites |
| **[Fixture Expert](rspec-fixture-expert.md)** | Rails fixtures management | Setting up test data |

## 💡 How to Use

### Basic Usage

1. **Start with the orchestrator** to analyze what type of tests you need:
```
@rspec-agent Help me write tests for my new payment processing feature
```

2. **Use specific agents** for targeted testing:
```
@rspec-model-specs-agent Write tests for my Order model validations
```

3. **Combine agents** for comprehensive coverage:
```
Use @rspec-request-specs-agent for the API and @rspec-system-specs-agent for the UI flow
```

### Example Workflows

#### 🆕 New Feature Development
```markdown
1. @rspec-tdd-advice-agent - Plan the testing approach
2. @rspec-model-specs-agent - Test the data layer
3. @rspec-request-specs-agent - Test the API/controller
4. @rspec-system-specs-agent - Test critical UI paths
```

#### 🔄 Refactoring Existing Tests
```markdown
1. @rspec-dry-agent - Identify duplication
2. @rspec-fixture-expert - Optimize test data
3. @rspec-isolation-testing-agent - Improve external service mocking
```

#### 📧 Email Feature
```markdown
1. @rspec-action-mailer-agent - Test email content and delivery
2. @rspec-active-job-agent - Test async email sending
3. @rspec-system-specs-agent - Test email trigger flows
```

## 🎓 Best Practices

### ✅ Do's
- Start with the orchestrator agent for complex features
- Use fixtures for consistent test data
- Keep tests focused and readable
- Test behavior, not implementation
- Run tests with `--fail-fast` during development

### ❌ Don'ts
- Don't modify `rails_helper.rb` or `spec_helper.rb`
- Don't add new testing gems - use what's configured
- Don't test Rails framework functionality
- Don't write performance tests unless specifically needed
- Don't over-abstract - favor clarity over DRY in tests

## 📝 Example Commands

```bash
# Run specific test
rspec spec/models/user_spec.rb

# Run with fail-fast
rspec --fail-fast

# Run specific line
rspec spec/models/user_spec.rb:42

# Run only model specs
rspec spec/models

# Run with documentation format
rspec --format documentation
```

## 🏗️ Project Structure

```
your-rails-app/
├── spec/
│   ├── models/          # Model specs
│   ├── requests/        # Request/controller specs
│   ├── system/          # System/feature specs
│   ├── jobs/            # ActiveJob specs
│   ├── mailers/         # ActionMailer specs
│   ├── fixtures/        # Test data fixtures
│   └── support/         # Shared contexts, helpers
└── rspec-*.md          # Agent instruction files
```

## 🤝 Contributing

Have ideas for improving these agents? Found a testing pattern that should be included? 

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-agent`)
3. Commit your changes (`git commit -m 'Add amazing agent'`)
4. Push to the branch (`git push origin feature/amazing-agent`)
5. Open a Pull Request

## 📚 Resources

- [RSpec Documentation](https://rspec.info/)
- [Better Specs](https://www.betterspecs.org/) - RSpec best practices
- [Rails Testing Guide](https://guides.rubyonrails.org/testing.html)
- [Effective Testing with RSpec 3](https://pragprog.com/titles/rspec3/effective-testing-with-rspec-3/)

## 📄 License

MIT License - feel free to use these agents in your projects!

## 🙏 Acknowledgments

These agents are built on community best practices from:
- The Rails and RSpec communities
- Testing patterns from [Thoughtbot](https://thoughtbot.com/blog)
- [Evil Martians](https://evilmartians.com/) testing guides
- Countless open source Rails projects

---

**Made with ❤️ for the Rails testing community**

*Transform your AI assistant into an RSpec expert today!*