# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a collection of specialized RSpec testing agents for Ruby on Rails applications. Each agent is designed to handle specific testing scenarios, following best practices and maintaining consistency across test suites.

## Common Development Tasks

### Running Individual Tests
```bash
# Run a specific spec file
rspec spec/path/to/file_spec.rb

# Run with fail-fast to stop at first failure
rspec --fail-fast

# Run a specific example by line number
rspec spec/path/to/file_spec.rb:42
```

## Architecture

### Agent Organization

The repository contains specialized RSpec testing agents, each with a specific focus:

**Orchestration:**
- `rspec-rails-agent.md` - Master RSpec Rails agent that analyzes testing needs and delegates to specialized agents

**Core Testing Agents:**
- `rspec-model-specs-agent.md` - ActiveRecord models, validations, scopes, methods
- `rspec-request-specs-agent.md` - Controllers, API endpoints, HTTP-level testing
- `rspec-system-specs-agent.md` - Full UI workflows, JavaScript interactions

**Rails Component Agents:**
- `rspec-active-job-agent.md` - Background jobs, async processing
- `rspec-action-mailer-agent.md` - Email functionality and delivery
- `rspec-active-storage-agent.md` - File uploads and attachments
- `rspec-actioncable-agent.md` - WebSocket and real-time features

**Testing Strategy Agents:**
- `rspec-tdd-advice-agent.md` - Test-first development coaching
- `rspec-isolation-testing-agent.md` - Mocking, stubbing, external dependencies
- `rspec-dry-agent.md` - Test refactoring, reducing duplication
- `rspec-fixture-expert.md` - Rails fixtures management and best practices

### Agent Selection Workflow

1. **Main Agent First**: Always start with `rspec-rails-agent.md` to determine the appropriate testing strategy
2. **Fixture Setup**: Review `rspec-fixture-expert.md` for fixture patterns and existing test data
3. **Isolation Strategy**: Consider `rspec-isolation-testing-agent.md` for external dependencies
4. **Component-Specific**: Use specialized agents based on Rails component being tested
5. **DRY Refactoring**: Apply `rspec-dry-agent.md` patterns to avoid duplication

### Testing Principles

- **Never modify** `rails_helper.rb` or `spec_helper.rb` - assume RSpec is already configured
- **Never add testing gems** to the Gemfile - use existing setup
- **Use fixtures** as the primary test data strategy (not factories)
- **Keep scope minimal** - start with essential tests, avoid edge cases unless requested
- **No performance testing** unless explicitly required
- **Balance DRY vs DAMP** - prioritize readability over abstraction

### Test Organization Patterns

```
Question: What am I testing?
├── Data & Business Logic → Model Specs Agent
├── HTTP & Controllers → Request Specs Agent
├── User Interface → System Specs Agent
├── Background Processing → ActiveJob Agent
├── Email → ActionMailer Agent
├── File Uploads → ActiveStorage Agent
├── Real-time Features → ActionCable Agent
└── Test Quality → Strategy Agents (TDD, DRY, Isolation)
```

### Common Testing Patterns

**New Feature Development:**
1. TDD Advice Agent for planning
2. Model Specs for data layer
3. Request Specs for API/controllers
4. System Specs for critical UI paths

**API Development:**
1. Request Specs for endpoints
2. ActiveJob for async processing
3. ActionMailer for notifications

**Full-Stack Feature:**
1. System Specs for user journey
2. Request Specs for controller logic
3. Model Specs for data layer
4. DRY Agent for cleanup