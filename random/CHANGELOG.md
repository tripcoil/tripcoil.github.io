# Relay Tools Changelog

## Issues Found and Fixed in Original Files

### capacitor-bank-calculator.html
**BUG FIXED: Reactor unit conversion error**
- Original code divided by 1000 twice, producing microhenries instead of henries
- `updateReactor()` function was: `phases[phase].reactor = parseFloat(value) / 1000 / 1000;`
- Fixed to: `phases[phase].reactor = parseFloat(value) / 1000;`
- Impact: Reactor inductance was 1000x too small, causing incorrect voltage/current calculations

**ADDED: MVAR display**
- Original only showed VAR, added MVAR for utility-scale banks

### mho-circles-visualizer.html
**FIXED: Misleading title**
- Original title: "SEL 311C Mho Circles Visualizer"
- Changed to: "Mho Distance Relay Visualizer"
- Reason: Tool is generic, not SEL-specific. The k0 calculation method works for any relay.

**ADDED: Show work section**
- Added expandable section showing k0 calculation steps

### sync-scope-trainer.html
**ADDED: Missing critical features**
- Slip frequency display (was not shown, essential for syncing)
- Phase angle readout
- Voltage difference percentage
- Sync status indicator ("IN SYNC" / "NOT IN SYNC")
- Synchronizing guidelines reference

**FIXED: No visual feedback**
- Original had no indication of whether it was safe to close breaker

### transformer-phasing-tool.html
**ADDED: Vector group display**
- Original showed phasors but didn't display IEEE/IEC vector group (Dy1, Yd11, etc.)
- Added vector group designation with explanation

**ADDED: Phase relationship table**
- Shows bushing-to-phase mapping and angles

**IMPROVED: Phase shift logic documentation**
- Added explanation section clarifying D-Y leads/lags convention per IEEE C57.12.00

### multi-phasor-visualizer.html
**UI IMPROVEMENTS ONLY**
- No calculation bugs found
- Applied consistent dark theme
- Improved slider layout and controls

---

## New Tools Added

1. **symmetrical-components.html** - ABC ↔ Sequence conversion
2. **fault-current-calculator.html** - 3φ, SLG, LL, LLG fault analysis
3. **tcc-curve-plotter.html** - IEEE C37.112 overcurrent curves
4. **per-unit-converter.html** - Actual ↔ p.u. with base change
5. **ct-saturation-calculator.html** - IEEE C37.110 CT analysis
6. **distance-relay-settings.html** - Zone reaches and k0 settings

---

## Theme Changes (All Files)

- Light theme → Dark engineering theme (#0f172a background)
- Added mobile responsive layouts
- Added print-friendly styles
- Added "Show Work" expandable sections with formulas
- Consistent color scheme across all tools
