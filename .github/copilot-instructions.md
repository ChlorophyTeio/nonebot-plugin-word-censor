# Copilot Instructions for `nonebot_plugin_word_censor`

## 🧠 Project Architecture & Concept
- **Purpose**: A NoneBot2 plugin acting as an **outgoing message firewall**. It intercepts bot replies to prevent sensitive content leaks.
- **Core Mechanism**: Uses `Bot.on_calling_api` global hook to inspect `send_msg`, `send_group_msg`, and `send_private_msg` calls.
- **Interception Logic**: Unlike event matchers, this plugin raises `MockApiException` to cancel the API call when a blacklist match occurs. This simulates a "failed" API call to the bot framework.

## 📂 Code Structure & Key Files
- **`nonebot_plugin_word_censor/__init__.py`**:
  - `_check_black_list`: The core hook function.
  - `_load_blacklist` / `_save_blacklist_to_file`: JSON persistence handling.
  - `wb_add`, `wb_del`, etc.: Command matchers for dynamic configuration.
- **`config.py`**: Pydantic `Config` model definition.
- **`data/send_word_blacklist.json`**: Runtime storage for blocked words/regex.

## 🛠 Developer Workflows

### Configuration & Data
- **Data Persistence**: Uses `nonebot-plugin-localstore` to manage the blacklist file (`send_word_blacklist.json`). Path resolution is handled automatically.
- **Data Format**:
  ```json
  {
    "blacklist": ["badword1", "badword2"],
    "regex_blacklist": ["^pattern.*$"]
  }
  ```
- **Hot Reloading**: `_load_blacklist()` is called on startup and via the `refresh` command.

### Coding Conventions
- **Internal Scope**: Use `_` prefix for helper functions (`_mask_word`, `_compile_regex_list`) and module-level globals (`_BLACKLIST_WORDS`).
- **Exception Handling**: Use `MockApiException` specifically for blocking actions.
- **Regex**: Always compile user-provided regex with `re.IGNORECASE` for performance and consistency. Store compiled patterns in `_COMPILED_REGEX`.

### Testing & Verification
- **Command Testing**: Verify commands (`add`, `del`) by checking if they update both the in-memory lists AND the JSON file.
- **Interception Testing**: To test the core logic, you must simulate a `bot.call_api` invocation with a message containing blacklisted content and assert `MockApiException` is raised.

## 🔗 Integration Points
- **NoneBot Hooks**: Relies heavily on `get_driver().on_startup` and `Bot.on_calling_api`.
- **Superuser Only**: All management commands are strictly restricted to `SUPERUSER`.
