# End-of-Game Decision Tool - Logic Documentation

## Overview
This tool provides strategic recommendations for five key end-of-game scenarios in college football. All logic is simplified for demonstration purposes and would need refinement with actual analytics data.

## Input Variables
- **Time Remaining**: 0-15 minutes (in 15-second increments)
- **Score Differential**: -21 to +21 points
- **Possession**: Leading team, Trailing team, or Tied game
- **Field Position**: -50 to +50 (precise yard line - negative = opponent territory, positive = own territory, 0 = 50-yard line)
- **Down & Distance**: 1st, 2nd, 3rd, or 4th down (assumes 10 yards to go)
- **Timeouts**: 0-3 for each team

## Visual Highlighting System
The tool uses dynamic highlighting to show which scenarios are relevant to the current game state:
- **Current situation rows**: Highlighted in blue with border and glow
- **Other situation rows**: Dimmed to 40% opacity
- **Score slider**: Controls highlighting in Clock Management matrix
- **Time slider**: Controls highlighting in After Score, Kickoff, and 2-Point matrices  
- **Field position slider**: Controls highlighting in 4th Down matrix

---

## 1. Clock Management Matrix

### Matrix Structure
- **Rows**: Score situations (Up 14+, Up 7-13, Up 1-6, Tied, Down 1-6, Down 7-13, Down 14+)
- **Columns**: Time ranges (0-1 min, 1-2 min, 2-3 min, 3-4 min, 4-5 min, 5-7 min, 7-10 min, 10+ min)
- **Highlighting**: Score differential slider determines which row is highlighted

### Decision Logic

#### Leading Team Scenarios ("Up" situations)
**KNEEL (Victory Formation)**
- Leading by ANY amount with ≤1 minute remaining
- Leading by 7+ points with ≤1:30 remaining  
- Leading by 14+ points with ≤2 minutes remaining

**RUN CLOCK**
- Leading team in 0-3 minute time slots (when not kneeling)
- Priority: Force opponent timeouts, control clock

**RUN SAFE**
- Leading team in 4-7 minute time slots
- Conservative running plays, avoid turnovers

**BALANCED**
- Leading team with 7+ minutes remaining
- Mix of run/pass, normal game management

#### Trailing Team Scenarios ("Down" situations)
**HAIL MARY**
- Trailing in 0-1 minute slot
- Desperation passing plays

**2-MIN DRILL**
- Trailing in 1-2 minute and 2-3 minute slots
- Aggressive passing, clock management, use timeouts

**HURRY UP**
- Trailing in 3-5 minute slots
- Up-tempo offense, selective passing

**AGGRESSIVE**
- Trailing with 5+ minutes
- Aggressive play-calling to make up deficit

#### Tied Game Scenarios
**CAREFUL AGG**
- Tied game with ≤3 minutes
- Balanced aggression, avoid turnovers

**CONTROL**
- Tied game with 3+ minutes
- Maintain possession, field position focus

---

## 2. 4th Down Decision Matrix

### Matrix Structure
- **Rows**: Field positions (Own 1-20, Own 21-40, Own 41-50, 50 to Opp 40, Opp 39-20, Red Zone)
- **Columns**: Yards to go (1, 2, 3, 4, 5, 6-8, 9-12, 13-20, 21+, FG Range)
- **Highlighting**: Field position slider determines which row is highlighted based on these ranges:
  - Own 20 to Own 50 → "Own 1-20" row
  - Own 11 to 50-yard line → "Own 21-40" row  
  - 50-yard line to Opp 30 → "Own 41-50" row
  - Opp 40 to Opp 30 → "50 to Opp 40" row
  - Opp 39 to Opp 21 → "Opp 39-20" row
  - Inside Opp 20 → "Red Zone" row

### Decision Logic

#### Red Zone (Opponent 20 and closer)
- **GO**: 1-2 yards to go
- **FG**: 3+ yards to go

#### Own Territory (Own 1-40)
- **GO**: Only if 1 yard to go AND under 2 minutes remaining
- **PUNT**: All other situations

#### Midfield to Opponent 40
- **GO**: 1-3 yards to go
- **PUNT**: 4+ yards to go

### Field Position Interpretation
- **Positive numbers** (1-50): Own territory (Own 35, Own 20, etc.)
- **Zero**: 50-yard line
- **Negative numbers** (-1 to -50): Opponent territory (Opp 35, Opp 15, etc.)

### Considerations Not Fully Implemented
- Current game score impact
- Kicker accuracy by distance
- Weather conditions
- Team-specific conversion rates

---

## 3. After Score Decision Matrix

### Matrix Structure
- **Rows**: Time remaining (0-1 min, 1-2 min, 2-4 min, 4-8 min, 8+ min)
- **Columns**: Score differential after TD (Down 15+, Down 8-14, Down 1-7, Tied, Up 1-7, Up 8+)
- **Highlighting**: Time slider determines which row is highlighted

### Decision Logic

#### 2-Point Conversion ("2-PT")
- **Down 15+ points**: Go for 2 (need multiple scores anyway)
- **Down 8-14 points**: Go for 2 (makes next score meaningful)

#### Extra Point ("PAT")
- **All other situations**: Take the guaranteed point

### Time-Based Highlighting
- ≤1 minute → "0-1 min" row highlighted
- 1-2 minutes → "1-2 min" row highlighted
- 2-4 minutes → "2-4 min" row highlighted
- 4-8 minutes → "4-8 min" row highlighted
- 8+ minutes → "8+ min" row highlighted

### Notes
- Logic is oversimplified
- Real decisions should consider:
  - Specific score (down 8 vs down 9)
  - Time remaining impact
  - Team 2-point conversion success rates
  - Opponent's likely scoring scenarios

---

## 4. Kickoff Strategy Matrix

### Matrix Structure
- **Rows**: Time remaining (0-1 min, 1-2 min, 2-4 min, 4-8 min, 8+ min)  
- **Columns**: Score differential (Down 9+, Down 8, Down 3-7, Down 1-2, Tied, Leading)
- **Highlighting**: Time slider determines which row is highlighted (same logic as After Score matrix)

### Decision Logic

#### Onside Kick ("ONSIDE")
- **Down 9+ points**: Onside if ≤2-4 minutes remaining
- **Down 8 points**: Onside if ≤2-4 minutes remaining
- **Any deficit**: Onside if ≤1 minute remaining

#### Squib Kick ("SQUIB")
- **Leading**: Use squib kick if ≤2-4 minutes remaining
- Purpose: Limit return, force opponent to use clock

#### Normal Kick ("NORMAL")
- **All other situations**: Regular kickoff

### Missing Considerations
- Onside kick success rates
- Return coverage quality
- Field position trade-offs

---

## 5. 2-Point Conversion Decision Matrix

### Matrix Structure
- **Rows**: Game timing (Early Game, Mid Game, 4th Qtr, Under 5 min, Under 2 min)
- **Columns**: Score differential (Down 15+, Down 8-14, Down 1-7, Tied, Up 1-7, Up 8+)
- **Highlighting**: Time slider determines which row is highlighted based on game phase:
  - ≤2 minutes → "Under 2 min" row
  - 2-5 minutes → "Under 5 min" row  
  - 5-15 minutes → "4th Qtr" row (assuming 4th quarter context)
  - 15+ minutes → "Mid Game" row

### Decision Logic

#### Go for 2-Point ("2-PT")
- **Down 15+ AND 4th quarter or later**: Need multiple scores
- **Down 8-14 AND under 5 minutes**: Make next score count

#### Extra Point ("PAT")  
- **All other situations**: Take guaranteed point

### Game Phase Logic
The highlighting system interprets game phases based on time remaining:
- **Under 2 min**: Critical end-game decisions
- **Under 5 min**: Late 4th quarter pressure
- **4th Qtr**: General 4th quarter context
- **Mid Game**: Earlier in game with more time

### Oversimplifications
- Doesn't account for specific scores (e.g., down 15 vs down 16)
- Missing team success rate data
- No consideration of opponent scoring trends

---

## Analysis Generation

### Primary Strategy
Main recommendation with reasoning based on situation

### Alternative Approach  
Secondary consideration or contrarian view

### Key Insight
Strategic principle or critical factor

### Key Stats
Relevant statistics or probabilities (currently generic)

### Most Likely Outcomes
Success/failure scenarios

---

## Interactive Features

### Dynamic Highlighting System
- **Visual Focus**: Current situation highlighted in blue, others dimmed
- **Real-time Updates**: Highlighting changes as sliders/controls are adjusted
- **Context Awareness**: Each matrix highlights based on relevant game state variables

### Responsive Controls
- **Time Slider**: 0-15 minutes, affects highlighting in time-based matrices
- **Score Slider**: -21 to +21, affects Clock Management highlighting
- **Field Position Slider**: -50 to +50 yard lines, affects 4th Down highlighting
- **Dropdown Controls**: Possession, down, timeouts for additional context

### Matrix Navigation
- **Tab Switching**: Five different decision scenarios
- **Cell Interactions**: Click any cell for detailed analysis
- **Real-time Updates**: All matrices update when game state changes

---

## Technical Implementation

### Field Position Logic
```javascript
// Positive = Own territory, Negative = Opponent territory, 0 = 50-yard line
function formatFieldPosition(fieldPos) {
    if (fieldPos === 0) return "50";
    if (fieldPos > 0) return "Own " + fieldPos;
    return "Opp " + Math.abs(fieldPos);
}
```

### Highlighting Logic
```javascript
// Score-based highlighting for Clock Management
function isCurrentScoreSituation(situation) {
    const score = gameState.scoreDiff;
    if (situation === 'Up 14+') return score >= 14;
    if (situation === 'Up 7-13') return score >= 7 && score <= 13;
    // ... etc
}
```

### Dynamic Updates
1. User adjusts slider/control
2. `updateDisplay()` function called
3. Appropriate matrix regenerated with highlighting
4. Analysis section updates with current context

---

## Known Limitations & Areas for Enhancement

### Data Integration Needed
- **Kicker accuracy** by distance and weather
- **Team conversion rates** (4th down, 2-point, etc.)
- **Historical success rates** by situation
- **Opponent tendencies** and defensive statistics
- **Real-time game context** (injuries, momentum, etc.)

### Logic Refinements Required
- **More granular score scenarios** (down 8 vs down 9 is very different)
- **Dynamic timeout impact** on decisions
- **Precise field position impact** (exact yard line considerations)
- **Weather conditions** affecting kicking and passing
- **Team-specific factors** (offensive/defensive efficiency)

### Additional Scenarios to Consider
- **Safety situations** (intentional safety for better punt position)
- **Clock management** after change of possession
- **Defensive strategy** recommendations (prevent vs aggressive)
- **Special teams** considerations beyond kicking

### Broadcasting Enhancements
- **Probability percentages** for each recommendation
- **Historical precedent** examples
- **Quick-hit stats** for on-air use
- **Alternative outcome** probabilities

---

## Development Setup

### GitHub Repository
- **URL**: https://github.com/victorres11/endgame-analyzer
- **Live Tool**: https://victorres11.github.io/endgame-analyzer/
- **Documentation**: Available in repository

### Local Development (Cursor)
```bash
git clone https://github.com/victorres11/endgame-analyzer.git
cd endgame-analyzer
# Open in Cursor for AI-assisted development
```

### Deployment
- **Automatic**: GitHub Pages updates automatically on push to main branch
- **No build process**: Pure HTML/CSS/JavaScript, works immediately

---

## Color Coding System
- **Green**: Aggressive/Go-for-it strategies (KNEEL, GO, NORMAL KICK)
- **Yellow**: Moderate risk/Field goals (RUN SAFE, FG, SQUIB KICK)
- **Orange**: Risky but viable options (HURRY UP, CAREFUL AGG)
- **Red**: Conservative/High-risk strategies (2-MIN DRILL, PUNT, ONSIDE)
- **Purple**: Desperation plays (HAIL MARY)

This documentation should help reviewers understand exactly how each decision is made, what factors are considered, how the interactive highlighting works, and what areas need refinement for a production version.