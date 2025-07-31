# End-of-Game Decision Tool - Logic Documentation

## Overview
This tool provides strategic recommendations for five key end-of-game scenarios in college football. All logic is simplified for demonstration purposes and would need refinement with actual analytics data.

## Input Variables
- **Time Remaining**: 0-15 minutes (in 15-second increments)
- **Score Differential**: -21 to +21 points
- **Possession**: Leading team, Trailing team, or Tied game
- **Field Position**: Own 20, Own 35, Midfield, Opponent 35, Red Zone
- **Down & Distance**: 1st, 2nd, 3rd, or 4th down (assumes 10 yards to go)
- **Timeouts**: 0-3 for each team

---

## 1. Clock Management Matrix

### Matrix Structure
- **Rows**: Score situations (Up 14+, Up 7-13, Up 1-6, Tied, Down 1-6, Down 7-13, Down 14+)
- **Columns**: Time ranges (0-1 min, 1-2 min, 2-3 min, 3-4 min, 4-5 min, 5-7 min, 7-10 min, 10+ min)

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

### Decision Logic

#### 2-Point Conversion ("2-PT")
- **Down 15+ points**: Go for 2 (need multiple scores anyway)
- **Down 8-14 points**: Go for 2 (makes next score meaningful)

#### Extra Point ("PAT")
- **All other situations**: Take the guaranteed point

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

### Decision Logic

#### Go for 2-Point ("2-PT")
- **Down 15+ AND 4th quarter or later**: Need multiple scores
- **Down 8-14 AND under 5 minutes**: Make next score count

#### Extra Point ("PAT")  
- **All other situations**: Take guaranteed point

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
- **Field position nuances** (35-yard line vs 37-yard line)
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

## Technical Implementation Notes

### How the Logic Updates
1. User moves sliders or changes dropdowns
2. `updateDisplay()` function called
3. `generateCurrentMatrix()` rebuilds visible matrix
4. Each cell calls appropriate decision function (e.g., `getClockStrategy()`)
5. Decision functions use current `gameState` variables
6. Matrix colors/text update based on returned recommendations

### Color Coding
- **Green**: Aggressive/Go-for-it strategies
- **Yellow**: Moderate risk/Field goals
- **Orange**: Risky but viable options  
- **Red**: Conservative/Punting strategies
- **Purple**: Desperation plays (Hail Mary)

This documentation should help reviewers understand exactly how each decision is made and what factors are (or aren't) currently considered in the logic.
