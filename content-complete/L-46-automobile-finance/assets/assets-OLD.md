# Asset Specifications for L-46: Financiamiento de Automóviles - Compra vs. Arrendamiento

## Day 1 Assets

### 1. Buy vs. Lease Comparison Infographic
- **Purpose:** Comparación visual lado a lado de compra versus arrendamiento
- **Format/Inputs:** Static infographic (SVG or high-res PNG), responsive design
- **Expected Outputs:** Referencia visual clara que los estudiantes pueden guardar/capturar
- **Interaction Model:** Static display
- **Design Notes:**
  - Split-screen layout: COMPRA (izquierda) vs. ARRENDAMIENTO (derecha)
  - Use icons: Casa (propiedad) para compra, Calendario/Reloj (temporal) para arrendamiento
  - Key comparisons with visual indicators:
    - Pago mensual (mostrar tamaño relativo con símbolos $)
    - Estado de propiedad (marca de verificación vs. X)
    - Límites de millaje (símbolo ilimitado vs. velocímetro con límite)
    - Resultado final (ícono de auto que conservas vs. flecha de devolución)
    - Costo a largo plazo (gráfico de barras mostrando comparación de 6 años)
  - Color scheme: Azul para compra, Naranja para arrendamiento
  - Include callout boxes for key insights: "Después de 6 años: POSEES UN ACTIVO DE $12K" vs. "Después de 6 años: PAGASTE $30K, NO POSEES NADA"

### 2. Vehicle Depreciation Curve Visualization
- **Purpose:** Mostrar cómo los vehículos pierden valor con el tiempo
- **Format/Inputs:** Interactive graph (HTML5/JavaScript) with hover states
- **Expected Outputs:** Comprensión visual de las tasas de depreciación
- **Interaction Model:**
  - Hover over years to see exact value
  - Toggle between new vs. used vehicle depreciation curves
  - Input purchase price to see personalized depreciation
- **Design Notes:**
  - X-axis: Años 0-10
  - Y-axis: Valor del vehículo ($0 a $50,000)
  - Show two curves: Auto nuevo (caída inicial más pronunciada) vs. Auto usado de 3 años (curva más suave)
  - Annotate key milestones: "Año 1: -20-30% de valor", "Año 5: -60% de valor"
  - Shaded area under curve represents "valor perdido"
  - Allow students to input their vehicle's purchase price to generate custom curve

### 3. Total Cost of Ownership Calculator Preview
- **Purpose:** Adelanto mostrando desglose completo de costos para enganchar a los estudiantes
- **Format/Inputs:** Static infographic showing example breakdown
- **Expected Outputs:** Momento "Ajá" de que el pago mensual es una pequeña parte del costo total
- **Interaction Model:** Static (full calculator available in Day 2)
- **Design Notes:**
  - Center: "$399/mes" grande en negrita
  - Radiating out: Costos adicionales con íconos
    - Seguro: +$200/mes
    - Combustible: +$150/mes
    - Mantenimiento: +$50/mes
    - Registro: +${{STATE_REGISTRATION_ANNUAL}}/mes (específico del estado)
  - Bottom total: "COSTO REAL: $839/mes"
  - Percentage circle: "El pago del vehículo es solo el 48% del costo total"
  - Callout: "¿Qué más estás olvidando?"

### 4. Loan Term Comparison Chart
- **Purpose:** Mostrar cómo la duración del préstamo afecta el interés total pagado
- **Format/Inputs:** Animated bar chart (can be static with strong visual design)
- **Expected Outputs:** Visual claro de las diferencias en el costo de interés
- **Interaction Model:** Static or simple animation
- **Design Notes:**
  - Three bars showing 36-month, 60-month, 72-month loans for same $25,000 vehicle
  - Each bar split into two colors:
    - Principal (azul más oscuro): Igual para todos
    - Interés (rojo): Crece significativamente cuanto más largo el plazo
  - Numbers displayed on bars: Pago mensual, Total pagado, Interés total
  - Visual insight: "¡El préstamo a 72 meses cuesta $4,500 MÁS en intereses!"

### 5. Real-World Example Cards
- **Purpose:** Ilustrar los escenarios de Mia, James y Taylor del contenido del Día 1
- **Format/Inputs:** Digital cards or scrollable panels (mobile-friendly)
- **Expected Outputs:** Presentación atractiva de estudios de caso
- **Interaction Model:** Click/tap to expand each person's full story
- **Design Notes:**
  - Three cards, each with:
    - Ilustración del personaje (representación diversa)
    - Nombre y enfoque: "La Estrategia de Compra y Conservación de Mia"
    - Gráfico de línea de tiempo mostrando su recorrido de 6-7 años
    - Números clave destacados: Total pagado, Valor final, Costo neto
    - Emoji de resultado: 😊 (Mia), 😰 (James), 🎯 (Taylor)
  - Use color coding: Verde (buen resultado), Rojo (costoso), Amarillo (equilibrado)

## Day 2 Assets

### 1. Auto Finance Decision Calculator (PRIMARY SKILL BUILDER)
- **Purpose:** Herramienta interactiva para comparación integral de compra vs. arrendamiento
- **Format/Inputs:** Web application with multi-section form
- **Expected Outputs:** Comparación de escenarios lado a lado con visualizaciones
- **Interaction Model:** Multi-step wizard with progressive disclosure
- **Design Notes:**

  **Step 1: Vehicle Selection**
  - Input: Marca/modelo del vehículo (dropdown or free text)
  - Input: Precio nuevo ($)
  - Input: Precio usado (3 años) ($)
  - Input: Oferta de arrendamiento pago mensual ($)
  - Auto-lookup integration with KBB/Edmunds API (if available)

  **Step 2: Financing Details - BUY NEW**
  - Pago inicial ($ or %)
  - Plazo del préstamo (dropdown: 36, 48, 60, 72 months)
  - Tasa de interés (% - default: {{STATE_AVG_AUTO_LOAN_RATE_NEW}}%)
  - STATE-SPECIFIC INPUTS (auto-populated for {{STATE_NAME}}):
    - Tasa de impuesto sobre ventas: {{STATE_SALES_TAX}}% (editable)
    - Tarifa de registro: Inicial ${{STATE_REGISTRATION_INITIAL}}, Anual ${{STATE_REGISTRATION_ANNUAL}} (editable)

  **Step 3: Financing Details - BUY USED**
  - Pago inicial ($ or %)
  - Plazo del préstamo (dropdown: 24, 36, 48, 60 months)
  - Tasa de interés (típicamente más alta que nuevo, mostrar como % por encima de la tasa nueva)
  - Same state-specific inputs

  **Step 4: Leasing Details**
  - Plazo de arrendamiento (dropdown: 24, 36, 39 months)
  - Reducción del costo capitalizado/pago inicial ($)
  - Límite anual de millaje (dropdown: 10k, 12k, 15k)
  - Factor de dinero (o equivalente APR)
  - Tarifa de adquisición: ${{STATE_LEASE_ACQUISITION_FEE}} (editable, state-specific)
  - Tarifa de disposición ($)

  **Step 5: Ownership Costs** (same for all scenarios)
  - Seguro mensual: ${{STATE_INSURANCE_AVG_TEEN}} default (editable, state-specific)
  - Millaje anual recorrido
  - Eficiencia de combustible (MPG)
  - Precio de gasolina: ${{STATE_GAS_PRICE_CURRENT}}/galón default (editable, state-specific)
  - Mantenimiento anual estimado ($)

  **Step 6: Results & Comparison**
  - Display three columns: COMPRAR NUEVO | COMPRAR USADO | ARRENDAR
  - For each show:
    - Pago mensual
    - Total pagado en 6 años
    - Valor del vehículo a los 6 años
    - Costo neto (pagado - valor)
    - Costo por milla
    - Gráfico de posición de capital (gráfico de líneas con el tiempo)
  - Visual winner indicator (costo neto más bajo destacado en verde)
  - "What if" sliders:
    - Ajustar tasa de interés ±2%
    - Cambiar millaje anual ±5000
    - Extender análisis a 10 años
  - Download/print button for results
  - Save scenario button (localStorage or account-based)

  **Technical Specifications:**
  - Responsive design (mobile, tablet, desktop)
  - Real-time calculation (no page refresh)
  - Input validation (prevent unrealistic values)
  - Tooltips on all fields explaining terms
  - State: {{STATE_NAME}} (automatically set, fields pre-populated)

### 2. Loan Terms Analyzer
- **Purpose:** Aislar el impacto de la duración del plazo del préstamo en el costo total
- **Format/Inputs:** Side-by-side comparison tool
- **Expected Outputs:** Visualización clara del impacto del plazo
- **Interaction Model:** Input loan amount and see three term scenarios
- **Design Notes:**
  - Single input: Monto del préstamo ($)
  - Automatic calculation of three scenarios (36/60/72 months)
  - Output table showing:
    - Pago mensual
    - Interés total pagado (DESTACADO)
    - Monto total pagado
    - Años bajo el agua (debiendo > valor)
  - Amortization schedule view (expandable)
  - Visual: Gráfico de líneas mostrando saldo del préstamo vs. valor del vehículo con el tiempo
    - Región "bajo el agua" sombreada
  - Opportunity cost calculator integration: "Invertir la diferencia de pago"

### 3. Total Cost of Ownership Worksheet (Interactive + Printable)
- **Purpose:** Guía paso a paso para calcular los costos completos del vehículo
- **Format/Inputs:** Interactive web form + PDF worksheet version
- **Expected Outputs:** Desglose completo de costos de 6 años
- **Interaction Model:** Guided form with calculations
- **Printable Version Specifications:**
  - 2-page PDF worksheet
  - Page 1: Costos de Compra y Financiamiento
    - Precio del vehículo: $_______
    - Pago inicial: $_______
    - Monto del préstamo: $_______
    - Tasa de interés: _____%
    - Plazo del préstamo: ____ meses
    - Pago mensual: $_______
    - Total pagado durante el préstamo: $_______
    - Interés total pagado: $_______
  - Page 1 continued: Costos Estatales e Iniciales
    - Impuesto sobre ventas ({{STATE_NAME}}): {{STATE_SALES_TAX}}% = $_______
    - Registro/título: ${{STATE_REGISTRATION_INITIAL}}
    - Inspección inicial (si aplica): {{#if STATE_INSPECTION_REQUIRED}}${{STATE_INSPECTION_COST}}{{else}}N/A{{/if}}
  - Page 2: Costos Anuales Continuos (Años 1-6)
    - Renovación de registro: $_____ × 6 = $_______
    - Seguro: $_____ × 6 = $_______
    - Combustible: $_____ × 6 = $_______
    - Mantenimiento: $_____ × 6 = $_______
    - Estimación de reparaciones: $_______
  - Page 2 continued: Cálculo Final
    - Costos totales (suma de todo lo anterior): $_______
    - Valor estimado del vehículo en el año 6: $_______
    - COSTO NETO: $_______
    - Costo por milla (÷ millas totales): $_______

### 4. Vehicle Financing Decision Matrix (Printable)
- **Purpose:** Marco personalizado para tomar decisiones de financiamiento
- **Format:** 1-page PDF worksheet
- **Content Structure:**
  - Section 1: Tus Prioridades (clasificar 1-5)
    - [ ] Pago mensual más bajo
    - [ ] Costo total más bajo
    - [ ] Tener siempre un vehículo más nuevo
    - [ ] Sin pagos de auto eventualmente
    - [ ] Flexibilidad para cambiar de vehículo
  - Section 2: Tus Circunstancias Actuales
    - Ingreso anual: $_______
    - Ingreso neto mensual: $_______
    - Ahorros actuales: $_______
    - Millaje anual esperado: _______
    - Rango de puntaje crediticio: Excelente / Bueno / Regular / En construcción
    - Estabilidad laboral: Muy Estable / Estable / Incierta / Cambiando Pronto
  - Section 3: State-Specific Factors
    - Estado: {{STATE_NAME}}
    - Tasa de impuesto sobre ventas: {{STATE_SALES_TAX}}%
    - Tarifas de registro: Inicial ${{STATE_REGISTRATION_INITIAL}}, Anual ${{STATE_REGISTRATION_ANNUAL}}
    - Costo promedio de seguro: ${{STATE_INSURANCE_AVG_TEEN}}/mes
    - {{#if STATE_INSPECTION_REQUIRED}}Inspección requerida: ${{STATE_INSPECTION_COST}} anualmente{{else}}No se requiere inspección{{/if}}
  - Section 4: Resumen de Análisis (de las actividades del Día 2)
    - Escenario 1 (Comprar Nuevo) costo neto: $_______
    - Escenario 2 (Comprar Usado) costo neto: $_______
    - Escenario 3 (Arrendar) costo neto: $_______
  - Section 5: Mi Marco de Decisión
    - Enfoque recomendado: Comprar Nuevo / Comprar Usado / Arrendar
    - Pago mensual máximo: $_______
    - Plazo máximo del préstamo: ____ meses
    - Rango de precio objetivo: $_______ a $_______
    - Plan después del pago del préstamo: _________________
  - Section 6: Mis Reglas Personales
    - No voy a: _____________________________
    - Voy a priorizar: _____________________________
    - Mi cronograma: _____________________________

### 5. Scenario Cards (Activity 1)
- **Purpose:** Perfiles de vehículos estandarizados para análisis grupal
- **Format:** Digital cards + printable handouts
- **Printable Version:**
  - Three 1-page handouts (one per profile)
  - Each includes:
    - Foto/ilustración del vehículo
    - Especificaciones (año, marca, modelo, precio)
    - Parámetros financieros dados (pago inicial, términos del préstamo, etc.)
    - STATE-SPECIFIC COST TABLE (auto-populated for {{STATE_NAME}}):
      - Impuesto sobre ventas: {{STATE_SALES_TAX}}%
      - Registro: Inicial ${{STATE_REGISTRATION_INITIAL}}, Anual ${{STATE_REGISTRATION_ANNUAL}}
      - Promedio de seguro: ${{STATE_INSURANCE_AVG_TEEN}}/mes
    - Espacio de trabajo de cálculo:
      - Pagos de compra/arrendamiento: $_______
      - Tarifas estatales: $_______
      - Seguro (6 años): $_______
      - Combustible (6 años): $_______
      - Mantenimiento (6 años): $_______
      - Valor residual: $_______
      - COSTO NETO TOTAL: $_______
      - Costo por milla: $_______/milla

### 6. Opportunity Cost Calculator Supplement
- **Purpose:** Mostrar en qué podrían convertirse los pagos de auto invertidos
- **Format:** Simple calculator + visual chart
- **Inputs:**
  - Diferencia de pago mensual entre escenarios ($)
  - Período de inversión (años)
  - Tasa de retorno esperada (default 7%)
- **Outputs:**
  - Valor futuro de los pagos invertidos
  - Contribuciones totales vs. crecimiento total
  - Visual: Gráfico de crecimiento con el tiempo
- **Printable Version:**
  - Reference table showing common scenarios:
    - $200/mes por 6 años al 7% = $17,392
    - $400/mes por 6 años al 7% = $34,785
    - $500/mes por 10 años al 7% = $87,052
  - Formula explanation for manual calculation
  - Reflection prompts: "¿Qué podría significar este dinero para tu futuro?"

## Downloadable Resources (Printable PDFs)

### 1. State-Specific Cost Reference Sheet (CRITICAL)
- **Purpose:** Referencia rápida para todos los costos específicos del estado
- **Format:** 1-page auto-populated reference (state-specific)
- **Content for {{STATE_NAME}}:**
  - Nombre del estado: {{STATE_NAME}} ({{STATE_CODE}})
  - Tasa de impuesto sobre ventas en vehículos: {{STATE_SALES_TAX}}%
  - Tarifas de registro:
    - Título y registro inicial: ${{STATE_REGISTRATION_INITIAL}}
    - Renovación anual: ${{STATE_REGISTRATION_ANNUAL}}
  - Costos promedio de seguro por grupo de edad:
    - 16-19: ${{STATE_INSURANCE_AVG_TEEN}}/mes
    - 20-25: [calculated from state data]
    - 26-35: [calculated from state data]
  - Precio promedio de gasolina (actual): ${{STATE_GAS_PRICE_CURRENT}}/galón
  - Tasas promedio de préstamos para automóviles:
    - Autos nuevos: {{STATE_AVG_AUTO_LOAN_RATE_NEW}}%
    - Autos usados: {{STATE_AVG_AUTO_LOAN_RATE_USED}}%
  - Requisitos de emisiones/inspección: {{#if STATE_INSPECTION_REQUIRED}}Sí - ${{STATE_INSPECTION_COST}} anualmente{{else}}No{{/if}}
  - Programas específicos del estado: [To be researched per state]
  - Enlaces:
    - DMV del estado: {{STATE_DMV_URL}}
    - Herramienta de comparación de seguros: {{STATE_INSURANCE_COMPARISON_URL}}
    - Protección al consumidor: {{STATE_CONSUMER_PROTECTION_URL}}

### 2. Auto Loan Comparison Worksheet
- **Purpose:** Comparación lado a lado de diferentes opciones de préstamo
- **Format:** 2-page PDF
- **Content:**
  - Header: Detalles del vehículo y préstamo
  - Three-column comparison table (3 diferentes plazos de préstamo)
  - Rows for: Pago mensual, Interés total, Total pagado, Cronograma de capital
  - Visual breakeven chart (¿cuándo tienes capital positivo?)
  - Decision criteria checklist

### 3. Buy vs. Lease Decision Flowchart
- **Purpose:** Herramienta de decisión visual para determinar el mejor enfoque
- **Format:** 1-page infographic
- **Content:**
  - Start: "Elegir Financiamiento de Vehículo"
  - Decision nodes:
    - "¿Conduces >15k millas/año?" → Sí: COMPRAR
    - "¿Puedes mantener el vehículo 8+ años?" → Sí: COMPRAR
    - "¿Necesitas minimizar el pago mensual?" → Tal vez arrendar
    - "¿Quieres construir capital?" → COMPRAR
    - "¿Valoras tener siempre un auto nuevo?" → Tal vez arrendar
  - End nodes with recommendations and warnings
  - QR codes to online calculators

### 4. Vehicle Financing Glossary
- **Purpose:** Referencia rápida para términos clave
- **Format:** 2-page PDF booklet
- **Content:**
  - Lista A-Z de términos con definiciones
  - Íconos para referencia visual
  - Ejemplos para cada término
  - Explicaciones de fórmulas (APR, tasa de depreciación, costo por milla)

### 5. Negotiation Tips & Consumer Rights Guide
- **Purpose:** Guía práctica para interacciones con concesionarios
- **Format:** 2-page PDF
- **Content (Page 1):**
  - Investiga antes de ir (herramientas de precios)
  - Conoce tu puntaje crediticio
  - Obtén financiamiento pre-aprobado
  - Negocia el precio, luego los pagos
  - No te apresures—los concesionarios quieren la venta
  - Lee TODOS los documentos antes de firmar
- **Content (Page 2):**
  - Tus derechos bajo la ley de {{STATE_NAME}}
  - Protecciones de ley de limón (específicas de {{STATE_NAME}})
  - Períodos de cancelación de contrato
  - Qué hacer si te presionan
  - Recursos: Procurador General de {{STATE_NAME}} ({{STATE_CONSUMER_PROTECTION_URL}}), Consumer Financial Protection Bureau

## Asset Development Priority

**Phase 1 (Essential for Launch):**
1. Auto Finance Decision Calculator (primary skill builder)
2. State-Specific Cost Reference Sheet (critical for accuracy)
3. Vehicle Financing Decision Matrix (for personal strategy)
4. Total Cost of Ownership Worksheet (printable)

**Phase 2 (Enhances Experience):**
5. Buy vs. Lease Comparison Infographic
6. Loan Terms Analyzer
7. Scenario Cards for Activity 1

**Phase 3 (Nice to Have):**
8. Vehicle Depreciation Curve Visualization
9. Opportunity Cost Calculator
10. Downloadable guides and resources

## Technical Implementation Notes

**State-Specific System Requirements:**
- State determined at district/admin level during LMS setup
- All content automatically displays {{STATE_NAME}} data
- State data layer (JSON files) updated annually
- All calculators pre-populated with state-specific defaults (editable by user)
- Printable PDFs auto-populate with {{STATE_NAME}} data

**Accessibility:**
- All interactive tools must meet WCAG 2.1 AA standards
- Keyboard navigation required
- Screen reader compatible
- Color-blind friendly palettes
- Text alternatives for all graphics

**Data Sources:**
- Bankrate.com for interest rates
- AAA for fuel costs and maintenance estimates
- Kelly Blue Book / Edmunds for vehicle values
- State DMV websites for fees
- Insurance.com state averages

**Asset File Naming Convention:**
- `l46-{asset-name}-{version}.{ext}`
- Example: `l46-auto-calculator-v1.html`
- Example: `l46-state-costs-california-v1.pdf`
