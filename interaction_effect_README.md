# The effect in one corner: a three-factor interaction

*An interactive demonstration of how marginal analyses hide interaction effects.*

## What this shows

An outcome can look flat across every single factor you test and still collapse hard in the
one place where several conditions coincide. This page lets you watch that happen. You stack
three binary factors one at a time and see the gap between the selected subgroup and everyone
else grow from a dismissible dip into an unmistakable effect, an effect that was present in
the data the whole time and that no one-factor test would have caught.

**All data is synthetic**, generated in the browser from a fixed seed. It represents no real
study, population, or exposure. The point is the statistical pattern and the reasoning, both
of which transfer to any dataset where effects can depend on combinations of conditions.

## The setup

A 2x2x2 design: three binary factors (a group, a state, an exposure) and one continuous
outcome. The outcome sits at a baseline everywhere except the single triple-positive cell,
where it is suppressed. Everything else is baseline plus noise.

## How to read it

Start with one switch on. Testing that factor alone averages the suppressed cell together
with three unaffected ones, so the effect is diluted to a small dip that is easy to write off
as noise. Add the second factor and the suppressed cell becomes half of what you are looking
at, so the gap widens. Add the third and the selected subgroup is exactly the suppressed
cell, and the full effect appears. The factorial grid below the controls shows all eight
combinations at once, so you can see the controls simply walking you toward one corner.

## Why it matters

An interaction is not the sum of its parts. If an effect only exists where several conditions
meet, then marginal tests, one factor at a time, are structurally built to miss it, and so is
any model that assumes the factors act independently. When a subgroup matters, you have to
test the subgroup: interaction terms, stratified estimates, or a model that carries the
product terms. The effect that hid from three separate tests can be obvious the moment you
look at the corner where it lives.
