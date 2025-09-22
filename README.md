# APort Assurance Check Action

Check agent assurance levels and compliance to ensure agents meet security requirements.

## 🚀 Features

- **Assurance Validation** - Verify agent assurance levels (1-5)
- **Compliance Checking** - Ensure agents meet minimum requirements
- **Flexible Thresholds** - Configurable minimum assurance levels
- **Detailed Reporting** - Comprehensive assurance information
- **GitHub Integration** - Seamless integration with GitHub Actions

## 📋 Usage

### Basic Usage

```yaml
name: APort Assurance Check
on:
  push:
    branches: [main]

jobs:
  assurance-check:
    runs-on: ubuntu-latest
    steps:
      - name: APort Assurance Check
        uses: aporthq/assurance-check-action@v1
        with:
          agent-id: ${{ secrets.APORT_AGENT_ID }}
          min-assurance-level: '3'
```

### Advanced Usage

```yaml
name: Advanced Assurance Check
on:
  workflow_dispatch:
    inputs:
      agent_id:
        description: 'Agent ID to check'
        required: true
      min_level:
        description: 'Minimum assurance level'
        required: false
        default: '4'

jobs:
  assurance-check:
    runs-on: ubuntu-latest
    steps:
      - name: APort Assurance Check
        uses: aporthq/assurance-check-action@v1
        with:
          agent-id: ${{ github.event.inputs.agent_id }}
          min-assurance-level: ${{ github.event.inputs.min_level }}
          api-base: 'https://api.aport.io'
          fail-on-low-assurance: true
```

## 🔧 Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `agent-id` | The APort Agent ID to check assurance for | ✅ | - |
| `api-base` | The base URL for the APort API | ❌ | `https://api.aport.io` |
| `min-assurance-level` | Minimum required assurance level (1-5) | ❌ | `3` |
| `fail-on-low-assurance` | Whether the action should fail if assurance level is too low | ❌ | `true` |

## 📤 Outputs

| Output | Description |
|--------|-------------|
| `assurance_level` | The agent assurance level (1-5) |
| `assurance_method` | The assurance method used |
| `verified_at` | When the assurance was verified |
| `compliant` | Boolean indicating if the agent meets assurance requirements |

## 🏗️ Setup

### 1. Create APort Agent

1. Go to [APort Dashboard](https://aport.io)
2. Create a new agent
3. Complete assurance verification
4. Note the Agent ID

### 2. Set GitHub Secrets

Add the following secrets to your repository:

```bash
APORT_AGENT_ID=your-agent-id-here
```

## 📊 Assurance Levels

| Level | Description | Requirements |
|-------|-------------|--------------|
| 1 | Basic | Email verification |
| 2 | Verified | Phone verification |
| 3 | Trusted | Identity verification |
| 4 | Certified | Professional verification |
| 5 | Enterprise | Organization verification |

## 🔍 Assurance Methods

- **Email Verification** - Basic email confirmation
- **Phone Verification** - SMS or call verification
- **Identity Verification** - Government ID verification
- **Professional Verification** - Professional license verification
- **Organization Verification** - Company verification

## 📚 Examples

### Check Before Deployment

```yaml
name: Deploy with Assurance Check
on:
  push:
    branches: [main]

jobs:
  check-assurance:
    runs-on: ubuntu-latest
    steps:
      - name: Check Agent Assurance
        uses: aporthq/assurance-check-action@v1
        with:
          agent-id: ${{ secrets.APORT_AGENT_ID }}
          min-assurance-level: '4'
          fail-on-low-assurance: true

  deploy:
    needs: check-assurance
    runs-on: ubuntu-latest
    steps:
      - name: Deploy Application
        run: echo "Deploying with high-assurance agent"
```

### Conditional Workflow

```yaml
name: Conditional Assurance Check
on:
  workflow_dispatch:

jobs:
  check-assurance:
    runs-on: ubuntu-latest
    steps:
      - name: Check Agent Assurance
        id: assurance
        uses: aporthq/assurance-check-action@v1
        with:
          agent-id: ${{ secrets.APORT_AGENT_ID }}
          min-assurance-level: '3'
          fail-on-low-assurance: false

  high-assurance-task:
    needs: check-assurance
    if: steps.assurance.outputs.assurance_level >= 4
    runs-on: ubuntu-latest
    steps:
      - name: High Assurance Task
        run: echo "Running high-assurance task"

  standard-task:
    needs: check-assurance
    if: steps.assurance.outputs.assurance_level < 4
    runs-on: ubuntu-latest
    steps:
      - name: Standard Task
        run: echo "Running standard task"
```

## 🚨 Troubleshooting

### Common Issues

1. **Agent not found**
   - Verify `APORT_AGENT_ID` is correct
   - Check agent exists in APort dashboard

2. **Low assurance level**
   - Complete additional verification steps
   - Contact APort support for assistance

3. **API errors**
   - Verify `api-base` URL is correct
   - Check network connectivity

### Debug Mode

Enable debug logging:

```yaml
- name: APort Assurance Check
  uses: aporthq/assurance-check-action@v1
  with:
    agent-id: ${{ secrets.APORT_AGENT_ID }}
    debug: true
```

## 📚 Examples

See the `example-repo/` directory for complete working examples.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📄 License

MIT License - see LICENSE file for details.

## 🆘 Support

- 📖 [Documentation](https://aport.io/docs)
- 💬 [Discord Community](https://discord.gg/aport)
- 🐛 [Issue Tracker](https://github.com/aporthq/assurance-check-action/issues)
- 📧 [Email Support](mailto:support@aport.io)
