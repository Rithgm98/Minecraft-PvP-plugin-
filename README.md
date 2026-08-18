# Coldblood - Professional Minecraft PvP Plugin

**Coldblood** is a professional, feature-rich PvP plugin designed for competitive Minecraft servers running Paper 1.21.11. It provides a complete duel system with ELO ranking, player statistics, configurable kits, and leaderboards.

## Features

### Core Features
- **Duel System**: Challenge other players with `/duel <player>`
- **Accept/Deny**: Accept or deny duel requests
- **ELO Ranking**: Competitive ranking system with configurable ELO calculations
- **Player Statistics**: Track wins, losses, kills, deaths, streaks, and more
- **Leaderboards**: Multiple leaderboard types (ELO, wins, kills, streaks)
- **PvP Kits**: Pre-configured kits (NoDebuff, Boxing, Sumo, BuildUHC, BedWars, Classic)
- **Server Management**: Set spawn locations and reload configurations

### Duel System
- Automatic opponent matching and arena selection
- Configurable countdown timer before combat
- Disconnect detection and handling
- Spectator mode support
- Win/loss tracking and automatic detection
- Automatic teleportation post-duel
- Combat protection during setup

### ELO Ranking System
- Starting ELO: 1000 (configurable)
- Dynamic ELO calculation based on opponent skill
- Rank tiers: Bronze → Silver → Gold → Platinum → Diamond → Master → Grandmaster → Coldblood
- Configurable ELO gains/losses
- Minimum ELO floor (prevents negative values)
- Rank-up notifications

### Statistics Tracking
- **Wins & Losses**: Overall match results
- **Kills & Deaths**: Combat statistics
- **Win Rate**: Calculated percentage
- **Win Streaks**: Current and best win streaks
- **ELO**: Current and highest ELO achieved

## Requirements

- **Minecraft Server**: Paper 1.21.11
- **Java**: Java 21 or higher
- **Database**: SQLite (default) or MySQL (optional)
- **Gradle**: For building the project

## Installation

### Pre-built JAR
1. Download `Coldblood-1.0.0.jar` from the releases page
2. Place the JAR file in your server's `plugins` directory
3. Restart your server
4. Configure the plugin in `plugins/Coldblood/`

### Building from Source
```bash
# Clone the repository
git clone https://github.com/Rithgm98/RavenCore.git
cd RavenCore

# Build the plugin
./gradlew shadowJar

# The JAR will be in build/libs/Coldblood-1.0.0.jar
```

## Commands

| Command | Permission | Description |
|---------|-----------|-------------|
| `/duel <player>` | `coldblood.duel` | Challenge a player to a duel |
| `/accept <player>` | `coldblood.duel` | Accept a duel request |
| `/deny <player>` | `coldblood.duel` | Deny a duel request |
| `/stats [player]` | `coldblood.stats` | View PvP statistics |
| `/rank [player]` | `coldblood.stats` | View player's ELO rank |
| `/leaderboard [type]` | `coldblood.leaderboard` | Display top players |
| `/kit [name]` | `coldblood.kit` | Select a PvP kit |
| `/spawn` | `coldblood.spawn` | Teleport to spawn |
| `/setspawn` | `coldblood.admin` | Set the server spawn |
| `/coldblood reload` | `coldblood.admin` | Reload configuration |
| `/coldblood version` | `coldblood.admin` | Show plugin version |

## Permissions

```yaml
coldblood.*              # All permissions
coldblood.admin         # Admin commands (default: op)
coldblood.duel          # Access duel system (default: true)
coldblood.stats         # View statistics (default: true)
coldblood.leaderboard   # View leaderboards (default: true)
coldblood.kit           # Select kits (default: true)
coldblood.spawn         # Use spawn command (default: true)
```

## Configuration

### config.yml
Main configuration file for database, ELO, duel, and server settings:

```yaml
database:
  type: sqlite  # sqlite or mysql
  file: plugins/Coldblood/data.db

elo:
  starting: 1000
  win-gain: 32
  loss-loss: 32
  min-elo: 0

duel:
  countdown: 10
  match-timeout: 600
  spectator-mode: true
```

### messages.yml
Customize all plugin messages and notifications.

### kits.yml
Define PvP kits with items and settings:

```yaml
kits:
  nodebuff:
    display-name: '&6NoDebuff'
    description: 'Classic PvP with no potion effects'
    items:
      - 'DIAMOND_HELMET'
      - 'DIAMOND_SWORD'
      - 'BOW:1'
      - 'ARROW:64'
      - 'GOLDEN_APPLE:8'
```

### arenas.yml
Configure duel arenas with player spawn points and center locations.

### ranks.yml
Define rank tiers based on ELO ranges:

```yaml
ranks:
  bronze:
    name: '&8Bronze'
    min-elo: 0
    max-elo: 1199
  gold:
    name: '&6Gold'
    min-elo: 1400
    max-elo: 1699
```

## Database Setup

### SQLite (Default)
No setup required! The plugin automatically creates `plugins/Coldblood/data.db`.

### MySQL
Update `config.yml`:

```yaml
database:
  type: mysql
  mysql:
    host: localhost
    port: 3306
    database: coldblood
    username: root
    password: your_password
```

Create the database:

```sql
CREATE DATABASE coldblood;
```

## Project Structure

```
Coldblood/
├── src/main/java/com/coldblood/
│   ├── Coldblood.java                 # Main plugin class
│   ├── commands/                       # Command handlers
│   │   ├── DuelCommand.java
│   │   ├── AcceptCommand.java
│   │   ├── StatsCommand.java
│   │   └── ...
│   ├── duel/                          # Duel management
│   │   ├── Duel.java
│   │   ├── DuelRequest.java
│   │   └── DuelManager.java
│   ├── elo/                           # ELO ranking
│   │   └── EloManager.java
│   ├── kits/                          # Kit management
│   │   ├── Kit.java
│   │   └── KitManager.java
│   ├── player/                        # Player data
│   │   ├── PlayerData.java
│   │   └── PlayerDataManager.java
│   ├── database/                      # Database operations
│   │   └── DatabaseManager.java
│   ├── leaderboard/                   # Leaderboards
│   │   └── LeaderboardManager.java
│   ├── listeners/                     # Event handlers
│   │   ├── DuelListener.java
│   │   ├── PlayerListener.java
│   │   ├── CombatListener.java
│   │   └── DeathListener.java
│   └── utils/                         # Utility classes
├── src/main/resources/
│   ├── plugin.yml
│   ├── config.yml
│   ├── messages.yml
│   ├── kits.yml
│   ├── arenas.yml
│   └── ranks.yml
├── build.gradle
├── settings.gradle
├── README.md
├── LICENSE
└── .gitignore
```

## Building

### Prerequisites
- Java 21 or higher
- Gradle 8.0 or higher

### Build Commands

```bash
# Build the plugin with dependencies
./gradlew buildPlugin

# Run tests (if applicable)
./gradlew test

# Clean build artifacts
./gradlew clean
```

### Output
The compiled plugin will be available at:
```
build/libs/Coldblood-1.0.0.jar
```

## Developer Information

### Architecture
- **Clean separation of concerns**: Commands, listeners, managers, and data handling
- **Thread-safe operations**: Uses ConcurrentHashMap for data storage
- **Configurable everything**: All messages, values, and settings via YAML
- **Async database operations**: Prevents blocking the Minecraft server thread
- **Event-driven system**: Uses Bukkit event listeners for all player interactions

### Adding Custom Kits

Edit `kits.yml`:

```yaml
kits:
  custom-kit:
    display-name: '&bCustom Kit'
    description: 'Your custom kit description'
    items:
      - 'DIAMOND_HELMET'
      - 'DIAMOND_SWORD'
      - 'COOKED_BEEF:32'
```

### Adding Custom Ranks

Edit `ranks.yml`:

```yaml
ranks:
  elite:
    name: '&e&lElite'
    min-elo: 4000
    max-elo: 999999
    color: '&e&l'
```

## Troubleshooting

### Plugin won't start
- Check Java version: Must be Java 21+
- Check server logs for error messages
- Verify `plugin.yml` is correct

### Database errors
- For SQLite: Ensure `plugins/Coldblood/` directory exists and is writable
- For MySQL: Verify database exists and credentials are correct

### Commands not working
- Check player has required permission
- Verify command is registered in `plugin.yml`
- Check server logs for errors

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Check existing documentation
- Review configuration examples

## Credits

Developed by **Rithgm98** for competitive Minecraft PvP servers.

## API Version

- **Paper**: 1.21.11
- **Java**: 21
- **Gradle**: 8.1+

---

**Coldblood** - Professional PvP at its finest.
