# Recruiter Shield

An intelligent agent that automatically detects, alerts, and blocks recruiter messages on LinkedIn before they reach your inbox.

## Features

- **Real-time Message Filtering**: Detects recruiter messages before they appear in your inbox
- **Smart Detection**: Uses AI to identify recruitment patterns and spam
- **Automatic Blocking**: Silently blocks unwanted recruiter communications
- **Alert System**: Notifies you when messages are blocked
- **Customizable Rules**: Configure which types of messages to block
- **Safe Mode**: Review blocked messages before permanent blocking

## Use Cases

- Reduce inbox noise from unsolicited recruitment
- Focus on networking with peers and industry professionals
- Maintain professional boundaries
- Save time by filtering irrelevant outreach

## Architecture

See [ARCHITECTURE.md](./ARCHITECTURE.md) for detailed technical documentation.

## Setup

1. Clone the repository
2. Install dependencies
3. Configure your LinkedIn credentials (securely)
4. Run the agent

## Usage

```bash
# Start the agent
npm start

# Run in monitoring mode
npm run monitor

# View blocked messages log
npm run logs
```

## Privacy & Security

- All credentials are stored locally
- No data is sent to external servers
- Open source and auditable
- Full control over what gets blocked

## License

MIT
