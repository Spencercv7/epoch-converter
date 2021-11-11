 # Epoch Converter ⏱

**Table of Contents**
- [Intro](#Hello!)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Challenges](#challenges)
- [Possible Improvements](#possible-improvements)

## Hello!
This was a fun project I undertook one afternoon during my time working at Bell. The project I was working on at the time for Bell required me to convert timestamps from epoch to standard date format. Not loving any of the converters on the Internet, I decided to do a single day code challenge where I created my own.

## Tech Stack
- [Svelte](https://svelte.dev/)
- [TypeScript](https://www.typescriptlang.org/)

## Getting Started
Click [here](https://epoch-converter.vercel.app/) to visit the site!

To run it locally, clone the repository, run "npm install" within the cloned directory, and lastly call "npm run dev" to deploy the development server. By default it should be live on http://localhost:5000/.

## Challenges
The site's most challenging component can be thought of a finite state machine. The two inputs (epoch input, standard date input) must display a value based on whatever is input to the other. For example, if the user inputs into the standard date input, the epoch date must be replaced with the equivalent value in epoch form, and vice versa. This presents some issue as it creates a dependency cycle.

The solution (as you can see in the code) is to detect which input is currently selected and ensure that the non-selected input is then updating its value based on the selected input.

## Possible Improvements
- Make more mobile friendly.
- Make completely accessible.
