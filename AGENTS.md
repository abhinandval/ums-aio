# Project Agent Guidelines

## Commit Convention

This project follows the [Conventional Commits](https://www.conventionalcommits.org/) specification for commit messages.

### Format
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that don't affect code meaning (formatting, semicolons, etc.)
- **refactor**: Code change that neither fixes a bug nor adds a feature
- **perf**: Performance improvement
- **test**: Adding or correcting tests
- **chore**: Changes to build process, tooling, dependencies, etc.

### Examples

```bash
feat: add jellyfin hardware acceleration support

fix(docker): correct volume permissions for qBittorrent

docs: update README with setup instructions

feat(sonarr): integrate with jellyfin notifications

chore(deps): update jellyfin to latest version

refactor: reorganize media folder structure
```

### Rules

1. Use lowercase for type and description
2. Keep description under 72 characters
3. Use present tense ("add" not "added")
4. Don't capitalize first letter of description
5. No period at end of description
6. Use body for additional context when needed
7. Reference issues in footer when applicable

### Breaking Changes

Add `BREAKING CHANGE:` in footer or use `!` after type/scope:

```
feat!: change default port mappings

BREAKING CHANGE: ports changed from 8xxx to 9xxx series
```

### Scopes (for this project)

- `docker`: Docker compose configuration
- `jellyfin`: Jellyfin service
- `sonarr`: Sonarr service
- `radarr`: Radarr service
- `qbittorrent`: qBittorrent service
- `prowlarr`: Prowlarr service
- `docs`: Documentation
- `config`: Configuration files
- `deps`: Dependencies/versions

## Project Structure

- `docker-compose.yml`: Main orchestration file
- `data/`: Service configurations (excluded from git)
- `media/`: Media storage (excluded from git)
- `.keep` files preserve directory structure

## Commands

```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down

# Update images
docker compose pull && docker compose up -d
```
