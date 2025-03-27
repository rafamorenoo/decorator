# Decorator

![image](https://github.com/user-attachments/assets/c58bebc3-7556-4e4a-b356-63eb8960b37f)

En este código que utiliza Balatro el valor de la variable config se va sobreescribiendo dependiendo del valor.


![image](https://github.com/user-attachments/assets/13a8f7f5-eaa4-4d8e-bcfb-be46666ec310)


# Posible solución


function CardArea:align_cards()

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



![image](https://github.com/user-attachments/assets/d07dc35f-d9e9-47a3-86aa-a870b2ec67e9)








-- Función para proteger los valores de las variables (inmutabilidad)
function protect_inmutable(obj)
    -- Crear una metatable que solo permita lectura
    local mt = {
        __index = function(table, key)
            return rawget(table, key)
        end,
        __newindex = function(table, key, value)
            -- Evitar que los valores de config sean modificados
            error("Modification of config is not allowed!")
        end
    }
    -- Aplicar la metatable a la tabla
    setmetatable(obj, mt)
    return obj
end



-- Suponiendo que 'self.config' es la configuración de la carta
self.config = protect_inmutable(self.config)
Esto protegerá el objeto self.config, de modo que si en algún lugar se intenta modificar cualquier valor dentro de config, Lua lanzará un error.




-- Decorador para manejar la lógica según el tipo de carta, usando self.config
function type_based_logic_decorator(func)
    return function(self, ...)
        -- Aseguramos que config esté protegido antes de usarlo
        self.config = protect_inmutable(self.config)

        -- Comprobar tipo de carta y ejecutar lógica correspondiente
        local config = self.config or {}

        if config.type == 'deck' then
            return func(self, 'deck', ...)
        elseif config.type == 'discard' then
            return func(self, 'discard', ...)
        elseif config.type == 'hand' then
            if G.STATE == G.STATES.TAROT_PACK or G.STATE == G.STATES.SPECTRAL_PACK then
                return func(self, 'hand_tarot', ...)
            else
                return func(self, 'hand_normal', ...)
            end
        elseif config.type == 'title' or (config.type == 'voucher' and #self.cards == 1) then
            return func(self, 'title_or_voucher_single', ...)
        elseif config.type == 'voucher' and #self.cards > 1 then
            return func(self, 'voucher_multiple', ...)
        elseif config.type == 'play' or config.type == 'shop' then
            return func(self, 'play_or_shop', ...)
        elseif config.type == 'joker' or config.type == 'title_2' then
            return func(self, 'joker_or_title_2', ...)
        elseif config.type == 'consumeable' then
            return func(self, 'consumeable', ...)
        else
            return func(self, 'default', ...)
        end
    end
end


-- Aplicamos el decorador para manejar la lógica según el tipo de carta usando self.config protegido
CardArea.align_cards = type_based_logic_decorator(function(self, type_of_card)
    -- Lógica común para todos los tipos de carta
    if (self == G.hand or self == G.deck or self == G.discard or self == G.play) and G.view_deck and G.view_de then
        return
    end

    -- Lógica específica según el tipo de carta
    if type_of_card == 'deck' then
        -- Lógica para el mazo
    elseif type_of_card == 'discard' then
        -- Lógica para la pila de descarte
    elseif type_of_card == 'hand_tarot' then
        -- Lógica cuando la mano está en estado TAROT o SPECTRAL
    elseif type_of_card == 'hand_normal' then
        -- Lógica cuando la mano no está en estado TAROT o SPECTRAL
    elseif type_of_card == 'title_or_voucher_single' then
        -- Lógica para títulos y vales con 1 carta
    elseif type_of_card == 'voucher_multiple' then
        -- Lógica para vales con más de 1 carta
    elseif type_of_card == 'play_or_shop' then
        -- Lógica para juego o tienda
    elseif type_of_card == 'joker_or_title_2' then
        -- Lógica para cartas tipo 'joker' o 'title_2'
    elseif type_of_card == 'consumeable' then
        -- Lógica para consumibles
    else
        -- Lógica para otros casos
    end

    -- Lógica común adicional
    for k, card in ipairs(self.cards) do
        -- Lógica para iterar sobre las cartas
    end

    if self.children.view_deck then
        -- Lógica adicional si hay una vista de mazo activa
    end
end)










