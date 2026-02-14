# [FIRST] Refactor: Improve Memory Manager API and Remove Code Hacks

## Description

This PR improves code quality by adding proper public methods to the `MemoryManager` class and eliminating direct access to private `_store` attributes throughout the codebase. It also cleans up documentation by removing outdated TODO comments and placeholders.

## Motivation

While reviewing the codebase, I found several instances where code was directly accessing `manager._store` with accompanying TODO/hack comments indicating this was not the intended design. The `MemoryManager` is meant to be a facade that encapsulates the underlying store implementation, but it was missing key public methods.

## Changes

### 1. Memory Manager API Enhancement

Added two new public methods to `MemoryManager`:

**`get_by_type(memory_type, limit, user_id)`**
- Retrieves memories filtered by type (LONG_TERM, DAILY, SESSION)
- Supports user scoping for memory isolation
- Replaces direct `_store.get_by_type()` calls

**`delete(entry_id)`**
- Deletes a memory entry by ID
- Returns boolean indicating success
- Replaces direct `_store.delete()` calls

### 2. Code Quality Improvements

Updated all locations using the `_store` hack:

- **dashboard.py**: Fixed memory API endpoints
  - Removed 5 lines of TODO/hack comments
  - Now uses proper public API
  
- **tools/builtin/memory.py**: Fixed memory forget tool
  
- **agents/pocketpaw_native.py**: Fixed native agent memory operations

### 3. Documentation Cleanup

- Removed TODO comment for Product Hunt badge
- Replaced "Discord: Coming Soon" with active GitHub Discussions link
- Provides users with an actual community channel

## Benefits

✅ **Better Encapsulation**: Store implementation is properly hidden  
✅ **Maintainability**: Future store changes won't break calling code  
✅ **Code Quality**: Eliminates technical debt and improves readability  
✅ **No Breaking Changes**: Only adds new methods, doesn't modify existing ones

## Testing

All modified Python files compile successfully without syntax errors:
- ✅ `src/pocketclaw/memory/manager.py`
- ✅ `src/pocketclaw/dashboard.py`
- ✅ `src/pocketclaw/tools/builtin/memory.py`
- ✅ `src/pocketclaw/agents/pocketpaw_native.py`

## Checklist

- [x] Code follows project conventions (async, protocol-oriented)
- [x] No breaking changes introduced
- [x] All modified files compile without errors
- [x] Follows Conventional Commits format
- [x] PR title starts with [FIRST]
- [x] Targets `dev` branch (as per CONTRIBUTING.md)

## Related Issues

This addresses technical debt mentioned in inline comments throughout the codebase regarding direct `_store` access being a "hack" that should be replaced with proper public methods.
