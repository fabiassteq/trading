// Soportes y resistencias con falso rompimientos

// Parámetros
period = input (14, "front.period", input.integer, 1   )
fn     = input (averages.ssma, "front.newind.average", input.string_selection, averages.titles)

// Variables
local averageFunction = averages [fn]
local previousBar = iBars(Symbol(), Period(), 1) - 1
local previousClose = iClose(Symbol(), previousBar)
local previousHigh = iHigh(Symbol(), previousBar)
local previousLow = iLow(Symbol(), previousBar)
local currentClose = iClose(Symbol(), iBars(Symbol(), Period(), 1))
local currentHigh = iHigh(Symbol(), iBars(Symbol(), Period(), 1))
local currentLow = iLow(Symbol(), iBars(Symbol(), Period(), 1))

// Soportes
local supports = []
for (int i = 0; i < iBars(Symbol(), Period(), 1); i++) {
    if (iBars(Symbol(), Period(), 1) - i > period && iBars(Symbol(), Period(), 1) - i > 1) {
        local support = previousClose
        for (int j = i; j < iBars(Symbol(), Period(), 1); j++) {
            if (currentClose > support) {
                support = currentClose
            }
        }
        supports.push(support)
    }
}

// Resistencias
local resistances = []
for (int i = 0; i < iBars(Symbol(), Period(), 1); i++) {
    if (iBars(Symbol(), Period(), 1) - i > period && iBars(Symbol(), Period(), 1) - i > 1) {
        local resistance = previousClose
        for (int j = i; j < iBars(Symbol(), Period(), 1); j++) {
            if (currentClose < resistance) {
                resistance = currentClose
            }
        }
        resistances.push(resistance)
    }
}

// Falso rompimientos
local falsosRompimientos = []
for (int i = 0; i < iBars(Symbol(), Period(), 1); i++) {
    if (iBars(Symbol(), Period(), 1) - i > period && iBars(Symbol(), Period(), 1) - i > 1) {
        local support = supports[i]
        local resistance = resistances[i]
        if (currentClose > support && currentClose < resistance) {
            falsosRompimientos.push(support)
        }
    }
}

// Mostrar resultados
Print("Soportes: ", supports)
Print("Resistencias: ", resistances)
Print("Falsos rompimientos: ", falsosRompimientos)
