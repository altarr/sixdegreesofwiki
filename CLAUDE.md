# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file vanilla JavaScript web game that challenges players to navigate from one Wikipedia article to another using only in-page links. The entire application is contained in `index.html` with embedded CSS and JavaScript.

## Architecture

**Single-file structure**: All code (HTML, CSS, JavaScript) lives in `index.html`. There is no build process, package manager, or external dependencies.

**Key components**:
- `state` object: Centralized game state tracking (playing status, titles, path, count, timer)
- Wikipedia API integration: Uses `https://en.wikipedia.org/w/api.php` for fetching article content and random articles
- Event-driven navigation: Click handlers intercept Wikipedia article links to track progress

**Core game flow**:
1. User inputs start and target Wikipedia URLs/titles
2. Game loads start article via Wikipedia API
3. Article links are transformed to track clicks (adding `data-wiki-title` attributes)
4. Each click increments counter, adds to path, and loads new article
5. Game ends when current article matches target article

## Development

**Running locally**:
```
Open `index.html` directly in a browser, or serve with any static server
```

**No build, test, or lint commands**: This is a static HTML file with no tooling.

## Code Patterns

**URL/Title normalization**:
- `ensureWikiUrlOrTitle(input)`: Accepts either full Wikipedia URLs or article titles
- `wikiTitleFromUrl(url)`: Extracts article title from Wikipedia URL
- `wikiUrlFromTitle(t)`: Converts title to Wikipedia URL
- `normalizeTitle(t)`: Standardizes title format (spaces, capitalization)

**Wikipedia API calls**:
- Article parsing: `action=parse&format=json&origin=*&prop=text&page={title}&redirects=`
- Random articles: `action=query&format=json&origin=*&list=random&rnnamespace=0&rnlimit=1`

**State management**:
The `state` object contains all game state. When modifying game logic:
- Always update `state` before DOM
- Use `renderPath()` to sync path display with state
- `resetGame(clearTargets)` resets state (optionally preserving start/target)

**Link transformation**:
After loading articles, all `/wiki/` links get `data-wiki-title` attribute. The article click handler uses this to distinguish Wikipedia navigation from external links.

## Query Parameters

The game supports auto-starting via URL parameters:
- `?start={title}&target={title}` - Pre-fills and auto-starts a challenge
- Used for sharing challenges between users
