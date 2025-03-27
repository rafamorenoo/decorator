# Decorator

![image](https://github.com/user-attachments/assets/c58bebc3-7556-4e4a-b356-63eb8960b37f)

En este código que utiliza Balatro el valor de la variable config se va sobreescribiendo dependiendo del valor.


![image](https://github.com/user-attachments/assets/da81298e-4ee3-43df-ba53-96ba66fa33fa)


# Posible solución


function CardArea:align_cards()
    -- Se asume que 'sombrero' es el equivalente a 'self.config'
    local sombrero = self.config

    if (self == G.hand or self == G.deck or self == G.discard or self == G.play) and G.view_deck and G.view_de then
        return
    end

    if sombrero.type == 'deck' then
        -- Lógica para el mazo
    end

    if sombrero.type == 'discard' then
        -- Lógica para la pila de descarte
    end

    if sombrero.type == 'hand' and (G.STATE == G.STATES.TAROT_PACK or G.STATE == G.STATES.SPECTRAL_PACK) then
        -- Lógica cuando la mano está en estado TAROT o SPECTRAL
    end

    if sombrero.type == 'hand' and not (G.STATE == G.STATES.TAROT_PACK or G.STATE == G.STATES.SPECTRAL_PACK) then
        -- Lógica cuando la mano no está en estado TAROT o SPECTRAL
    end

    if sombrero.type == 'title' or (sombrero.type == 'voucher' and #self.cards == 1) then
        -- Lógica para títulos y vales con 1 carta
    end

    if sombrero.type == 'voucher' and #self.cards > 1 then
        -- Lógica para vales con más de 1 carta
    end

    if sombrero.type == 'play' or sombrero.type == 'shop' then
        -- Lógica para juego o tienda
    end

    if sombrero.type == 'joker' or sombrero.type == 'title_2' then
        -- Lógica para cartas tipo 'joker' o 'title_2'
    end

    if sombrero.type == 'consumeable' then
        -- Lógica para consumibles
    end

    for k, card in ipairs(self.cards) do
        -- Lógica para iterar sobre las cartas
    end

    if self.children.view_deck then
        -- Lógica adicional si hay una vista de mazo activa
    end
end





# Riesgo de pasar el valor de una variable a otra

config = null;

sombrero = null;







![image](https://github.com/user-attachments/assets/468651fa-5b7d-475e-88c7-9fa686c88e75)



# Solución




![image](https://github.com/user-attachments/assets/0d3d2074-74a9-4485-b88e-0f5d08137682)






