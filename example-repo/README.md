# APort Assurance Check Example

This repository demonstrates how to use the APort Assurance Check GitHub Action to verify agent assurance levels and compliance.

## 🚀 Quick Start

### 1. Fork this Repository

Click the "Fork" button to create your own copy of this repository.

### 2. Set up APort Agent

1. Go to [APort Dashboard](https://aport.io)
2. Create a new agent
3. Complete assurance verification to achieve your desired level

### 3. Configure GitHub Secrets

Add the following secrets to your repository:

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Add the following secrets:

| Secret Name | Description | Example Value |
|-------------|-------------|---------------|
| `APORT_AGENT_ID` | Your APort Agent ID | `agent_1234567890abcdef` |

### 4. Test the Action

1. Push changes to the main branch
2. The action will automatically run and check assurance levels
3. Or trigger manually via **Actions** → **APort Assurance Check** → **Run workflow**

## 🔧 Configuration

### Assurance Levels

| Level | Description | Requirements |
|-------|-------------|--------------|
| 1 | Basic | Email verification |
| 2 | Verified | Phone verification |
| 3 | Trusted | Identity verification |
| 4 | Certified | Professional verification |
| 5 | Enterprise | Organization verification |

### Workflow Configuration

The workflow is configured to run on:

- **Push Events:** When changes are pushed to main branch
- **Manual Dispatch:** Can be triggered manually with custom parameters

## 📊 Usage Examples

### Basic Assurance Check

```yaml
name: Basic Assurance Check
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

### Conditional Workflow Based on Assurance

```yaml
name: Conditional Assurance Workflow
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

### Pre-deployment Assurance Check

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

## 🚨 Troubleshooting

### Common Issues

1. **"Agent not found" error**
   - Verify `APORT_AGENT_ID` secret is correct
   - Check agent exists in APort dashboard

2. **"Low assurance level" error**
   - Complete additional verification steps in APort dashboard
   - Contact APort support for assistance

3. **"API error" error**
   - Check network connectivity
   - Verify API endpoint is accessible

### Debug Mode

To enable debug logging, modify the workflow:

```yaml
- name: APort Assurance Check
  uses: aporthq/assurance-check-action@v1
  with:
    agent-id: ${{ secrets.APORT_AGENT_ID }}
    min-assurance-level: '3'
    debug: true  # Add this line
```

## 📚 Advanced Examples

### Multi-environment Assurance Check

```yaml
name: Multi-environment Assurance Check
on:
  push:
    branches: [main, staging, dev]

jobs:
  check-assurance:
    runs-on: ubuntu-latest
    steps:
      - name: Check Agent Assurance
        id: assurance
        uses: aporthq/assurance-check-action@v1
        with:
          agent-id: ${{ secrets.APORT_AGENT_ID }}
          min-assurance-level: ${{ github.ref == 'refs/heads/main' && '4' || '3' }}
          fail-on-low-assurance: false

      - name: Deploy to Production
        if: github.ref == 'refs/heads/main' && steps.assurance.outputs.assurance_level >= 4
        run: echo "Deploying to production"

      - name: Deploy to Staging
        if: github.ref == 'refs/heads/staging' && steps.assurance.outputs.assurance_level >= 3
        run: echo "Deploying to staging"

      - name: Deploy to Development
        if: github.ref == 'refs/heads/dev'
        run: echo "Deploying to development"
```

### Assurance Monitoring

```yaml
name: Assurance Monitoring
on:
  schedule:
    - cron: '0 */6 * * *'  # Every 6 hours

jobs:
  monitor-assurance:
    runs-on: ubuntu-latest
    steps:
      - name: Check Agent Assurance
        id: assurance
        uses: aporthq/assurance-check-action@v1
        with:
          agent-id: ${{ secrets.APORT_AGENT_ID }}
          min-assurance-level: '3'
          fail-on-low-assurance: false

      - name: Alert if Low Assurance
        if: steps.assurance.outputs.assurance_level < 3
        run: |
          echo "⚠️ Low assurance level detected: ${{ steps.assurance.outputs.assurance_level }}"
          # Add your alerting logic here
```

## 🤝 Contributing

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📄 License

MIT License - see LICENSE file for details.

## 🆘 Support

- 📖 [APort Documentation](https://aport.io/docs)
- 💬 [Discord Community](https://discord.gg/aport)
- 🐛 [Issue Tracker](https://github.com/aporthq/assurance-check-action/issues)
- 📧 [Email Support](mailto:support@aport.io)

## 🔗 Related

- [APort Dashboard](https://aport.io)
- [Assurance Check Action](https://github.com/aporthq/assurance-check-action)
- [APort Documentation](https://aport.io/docs)
