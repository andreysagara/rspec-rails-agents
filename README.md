# 🚀 RSpec Testing Agents for Rails

A collection of specialized AI agents designed to write comprehensive RSpec test suites for Ruby on Rails applications. Each agent focuses on specific testing scenarios, ensuring best practices and consistent test coverage across your Rails codebase.

## 🎯 What Are These Agents?

These are specialized prompts/instructions that transform your AI coding assistant (Claude Code, Cursor, GitHub Copilot, etc.) into expert RSpec test writers. Each agent has deep knowledge of specific Rails testing patterns and will help you write better tests faster.

## 🏗️ How To Use Agents

The current workflow is not exactly strict TDD. In order to write proper tests first, an architecture / API would need to be predictable or defined to test against. Since LLMs will be generating your code, the actual implementation you need to test against is non-deterministic unless you have defined an explicit and expected code outcome. Therefore, the effective use of these agents is to run them posthoc on the code that is being evaluated for production/use. YMMV. Do what works.

## 🤖 Available Agents

### 🎭 Master Orchestrator
- **[rspec-rails-agent.md](rspec-rails-agent.md)** - The main RSpec Rails agent that analyzes your testing needs and delegates to specialized agents

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

## Requirements

Your Rails application must have the following gems:

```ruby
group :test do
  gem 'rspec-rails'
  gem 'cuprite'
  gem 'vcr'
  gem 'simplecov', require: false    
end
```

For `simplecov` you should have:

In your `spec/spec_helper.rb`

```ruby
require 'simplecov'
SimpleCov.start 'rails'
```

For `simplecov` to work with system tests, in your `bin/rails` right under the `#!`:

```ruby
#!/usr/bin/env ruby

if ENV['RAILS_ENV'] == 'test'
  require 'simplecov'
  SimpleCov.start 'rails'
end
```

In your `spec/rails_helper.rb` you should have (or the equivelent of):

```ruby
require "capybara/cuprite"

Capybara.register_driver :cuprite do |app|
  Capybara::Cuprite::Driver.new(app,
                                window_size: [1400, 900],
                                browser_options: {
                                  'no-sandbox': nil,  # Required for Docker/CI
                                  'disable-gpu': nil, # Helpful in CI environments
                                  'disable-dev-shm-usage': nil
                                },
                                inspector: true, # Enable debugging in development
                                headless: true) # Set to false for debugging
end

# Set Cuprite as the JavaScript driver
Capybara.javascript_driver = :cuprite

# For system specs
RSpec.configure do |config|
  config.before(:each, type: :system, js: true) do
    driven_by :cuprite
  end
end

# Setup Fixtures in RSpec
RSpec.configure do |config|
  # Fixtures path
  config.fixture_paths = [
    Rails.root.join('spec/fixtures')
  ]

  # etc...
end
```

### Basic Usage

1. **Start with the orchestrator** to analyze what type of tests you need:
```
@rspec-rails-agent Help me write tests for my new payment processing feature
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

## 📦 Installation & Setup

### For Claude Code (Anthropic)

Claude Code supports two configuration approaches:

#### Option 1: Global Configuration (Apply to All Projects)

1. Clone this repository to your global claude agents:
```bash
mkdir ~/.claude/tmp
git clone --depth=1 https://github.com/aviflombaum/rspec-rails-agents.git ~/.claude/tmp/rspec-rails-agents
mkdir ~/.claude/agents/

cp ~/.claude/tmp/rspec-rails-agents/* ~/.claude/agents/
rm -rf ~/.claude/tmp/rspec-rails-agents
```

Now the agents are available in all your projects via the `claude` CLI (referencing the main `rspec-rails-agent` to orchestrate or a specific agent if you are writing tests for a specific layer).

#### Option 2: Project-Specific Configuration

1. Clone the agents into your Rails project:
```bash
cd /path/to/your/rails/project
git clone --depth=1 https://github.com/aviflombaum/rspec-agents.git .claude-agents
```

Now the agents are available for this project via the `claude` CLI (referencing the main `rspec-rails-agent` to orchestrate or a specific agent if you are writing tests for a specific layer).

2. Optionally create or update your project's `CLAUDE.md` file:
```bash
cat >> CLAUDE.md << 'EOF'

## RSpec Testing Agents

This project includes specialized RSpec testing agents in .claude-agents/
- Use .claude-agents/rspec-rails-agent.md as the main orchestrator
- Each agent handles specific Rails testing scenarios
- Always start with the orchestrator for complex testing needs
EOF
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
cat rspec-rails-agent.md >> .github/copilot-instructions.md
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
- [Everyday Rails Testing with RSpec](https://leanpub.com/everydayrailsrspec)
- Countless open source Rails projects

---

**Made with ❤️ for the Rails testing community**

*Transform your AI assistant into an RSpec expert today!*
