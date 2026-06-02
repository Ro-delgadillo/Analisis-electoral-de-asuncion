# Analisis-electoral-de-asuncion
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Análisis Electoral de Asunción</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@400;700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html, body {
            font-family: 'Inter', sans-serif;
            background: #f3f4f6;
            overflow-x: hidden;
        }

        h1, h2, h3, h4, h5, h6 {
            font-family: 'Merriweather', serif;
        }

        .seccion {
            display: none;
            min-height: 100vh;
            animation: fadeIn 0.8s ease-out;
        }

        .seccion.activa {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes slideDown {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(8px); }
        }

        /* HEADER */
        .header {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            background: rgba(255, 255, 255, 0.95);
            border-bottom: 1px solid #e5e7eb;
            padding: 0.75rem 2rem;
            z-index: 50;
            backdrop-filter: blur(10px);
            font-size: 0.75rem;
            color: #111827;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
        }

        .header-content {
            max-width: 100%;
            display: flex;
            flex-wrap: wrap;
            gap: 1.5rem;
            justify-content: space-between;
            align-items: center;
        }

        .header strong {
            font-weight: 600;
            color: #111827;
        }

        /* PROGRESS BAR */
        .progress-bar {
            position: fixed;
            top: 3.5rem;
            left: 0;
            right: 0;
            height: 4px;
            background: #e5e7eb;
            z-index: 40;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #3b82f6, #ef4444);
            transition: width 0.5s ease;
        }

        /* PORTADA */
        .portada {
            background: linear-gradient(135deg, #1e3a8a 0%, #991b1b 50%, #1e3a8a 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 6rem 2rem;
            text-align: center;
            position: relative;
        }

        .portada-contenido {
            max-width: 600px;
            z-index: 10;
        }

        .portada h1 {
            font-size: 3.5rem;
            font-weight: bold;
            color: white;
            margin-bottom: 1rem;
            line-height: 1.2;
        }

        .portada h2 {
            font-size: 1.875rem;
            color: white;
            margin-bottom: 2rem;
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        .portada p {
            font-size: 1.125rem;
            color: white;
            margin-bottom: 0.5rem;
            line-height: 1.6;
            opacity: 0.95;
        }

        .portada-stats {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 2rem;
            margin-top: 4rem;
            flex-wrap: wrap;
        }

        .stat-box {
            text-align: center;
        }

        .stat-box .numero {
            font-size: 3rem;
            font-weight: bold;
            color: white;
            display: block;
        }

        .stat-box .label {
            font-size: 0.875rem;
            color: #f0f9ff;
            margin-top: 0.5rem;
            font-weight: 500;
        }

        .divider {
            width: 1px;
            height: 80px;
            background: rgba(255, 255, 255, 0.2);
        }

        .bounce-indicator {
            position: absolute;
            bottom: 2rem;
            left: 50%;
            transform: translateX(-50%);
            color: white;
            font-size: 2rem;
            opacity: 0.8;
            animation: bounce 2s infinite;
        }

        /* SECCIONES GENERALES */
        .seccion {
            background: linear-gradient(180deg, #f9fafb 0%, #f3f4f6 100%);
            padding: 6rem 2rem 5rem;
        }

        .seccion-header {
            max-width: 80rem;
            margin: 0 auto 3rem;
            animation: slideDown 0.6s ease-out;
        }

        .seccion-header h1 {
            font-size: 3rem;
            font-weight: bold;
            color: #111827;
            margin-bottom: 1rem;
        }

        .seccion-header .divider-line {
            width: 5rem;
            height: 4px;
            background: #dc2626;
            margin-bottom: 2rem;
        }

        .grid-4 {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
            max-width: 80rem;
            margin: 0 auto 2rem;
        }

        .grid-2 {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 1.5rem;
            max-width: 80rem;
            margin: 0 auto;
        }

        .card {
            background: white;
            padding: 1.5rem;
            border-radius: 0.5rem;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
            border-top: 4px solid #3b82f6;
            transition: box-shadow 0.3s;
        }

        .card:hover {
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .card.red { border-top-color: #ef4444; }
        .card.green { border-top-color: #10b981; }
        .card.purple { border-top-color: #8b5cf6; }
        .card.orange { border-top-color: #f59e0b; }

        .card .numero {
            font-size: 2rem;
            font-weight: bold;
            color: inherit;
            margin-bottom: 0.5rem;
        }

        .card .label {
            font-size: 0.875rem;
            color: #6b7280;
        }

        .chart-container {
            background: white;
            padding: 2rem;
            border-radius: 0.5rem;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
            position: relative;
            height: 400px;
            margin-bottom: 2rem;
        }

        .chart-container h3 {
            font-size: 1.125rem;
            color: #1f2937;
            margin-bottom: 1rem;
            font-weight: 600;
        }

        .callout {
            padding: 1.5rem;
            border-left: 4px solid;
            margin: 2rem auto;
            max-width: 80rem;
            border-radius: 0.25rem;
        }

        .callout.blue {
            background: #eff6ff;
            border-color: #2563eb;
            color: #1f2937;
        }

        .callout.red {
            background: #fef2f2;
            border-color: #dc2626;
            color: #1f2937;
        }

        .callout.orange {
            background: #fffbeb;
            border-color: #f59e0b;
            color: #1f2937;
        }

        .callout.green {
            background: #f0fdf4;
            border-color: #16a34a;
            color: #1f2937;
        }

        .callout strong {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
        }

        /* DARK SECTION */
        .dark-section {
            background: linear-gradient(180deg, #111827 0%, #1f2937 100%);
            color: white;
            padding: 6rem 2rem 5rem;
        }

        .dark-section .seccion-header h1 {
            color: white;
        }

        .dark-section .seccion-header .divider-line {
            background: #ef4444;
        }

        .hallazgo-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            padding: 2rem;
            border-radius: 0.5rem;
            margin: 0 auto 1.5rem;
            max-width: 80rem;
            display: flex;
            gap: 1.5rem;
        }

        .hallazgo-card .numero {
            font-size: 2.5rem;
            font-weight: bold;
            min-width: 3rem;
            flex-shrink: 0;
        }

        .hallazgo-card h3 {
            font-size: 1.5rem;
            color: white;
            font-weight: 600;
            margin-bottom: 0.75rem;
        }

        .hallazgo-card p {
            color: white;
            opacity: 0.95;
            line-height: 1.6;
        }

        /* FUENTES */
        .fuente-box {
            background: white;
            padding: 1.5rem;
            border-radius: 0.5rem;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
            margin-bottom: 1.5rem;
            max-width: 80rem;
            margin-left: auto;
            margin-right: auto;
        }

        .fuente-box h3 {
            font-size: 1.125rem;
            font-weight: 600;
            color: #1f2937;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .fuente-box .contenido {
            color: #4b5563;
            font-size: 0.95rem;
            line-height: 1.6;
        }

        .fuente-box ul {
            margin-left: 1.5rem;
            margin-top: 0.75rem;
        }

        .fuente-box li {
            margin-bottom: 0.5rem;
        }

        .fuente-box .subtitulo {
            font-weight: 600;
            color: #1f2937;
            margin-top: 1rem;
            margin-bottom: 0.5rem;
        }

        /* NAVEGACION */
        .nav-footer {
            position: fixed;
            bottom: 2rem;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(255, 255, 255, 0.95);
            padding: 1rem 1.5rem;
            border-radius: 9999px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
            display: flex;
            gap: 1.5rem;
            align-items: center;
            z-index: 1000;
            border: 1px solid #e5e7eb;
        }

        .nav-button {
            background: none;
            border: none;
            cursor: pointer;
            padding: 0.5rem;
            border-radius: 9999px;
            transition: background 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #1f2937;
        }

        .nav-button:hover:not(:disabled) {
            background: #f3f4f6;
        }

        .nav-button:disabled {
            opacity: 0.4;
            cursor: not-allowed;
        }

        .nav-button svg {
            width: 24px;
            height: 24px;
            stroke: currentColor;
        }

        .nav-info {
            text-align: center;
            min-width: 200px;
        }

        .nav-info .paso {
            font-size: 0.875rem;
            color: #1f2937;
            font-weight: 600;
        }

        .nav-info .titulo {
            font-size: 0.75rem;
            color: #4b5563;
            margin-top: 0.25rem;
            font-weight: 500;
        }

        /* RESPONSIVE */
        @media (max-width: 768px) {
            .header-content {
                flex-direction: column;
                gap: 1rem;
                font-size: 0.65rem;
            }

            .header {
                padding: 0.5rem 1rem;
            }

            .portada h1 {
                font-size: 2rem;
            }

            .portada h2 {
                font-size: 1.5rem;
            }

            .portada-stats {
                flex-direction: column;
                gap: 1rem;
            }

            .divider {
                display: none;
            }

            .seccion-header h1 {
                font-size: 2rem;
            }

            .grid-4, .grid-2 {
                grid-template-columns: 1fr;
            }

            .nav-footer {
                bottom: 1rem;
                padding: 0.75rem 1rem;
                gap: 1rem;
            }

            .nav-info {
                min-width: 150px;
            }

            .hallazgo-card {
                flex-direction: column;
                text-align: center;
            }

            .hallazgo-card .numero {
                min-width: auto;
            }
        }

        .progress-bar {
            top: calc(3.5rem + 1px);
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="header-content">
            <div>
                <strong>Fuente:</strong> Tribunal Superior de Justicia Electoral (TSJE) | <strong>Ley:</strong> N° 5282/2014
            </div>
            <div>
                <strong>Autor:</strong> Rodrigo Alejandro Delgadillo Ortiz | 📧 ro.delgadillo@outlook.com | 📱 +595991944778
            </div>
        </div>
    </div>

    <div class="progress-bar">
        <div class="progress-fill" id="progressFill"></div>
    </div>

    <!-- PORTADA -->
    <div class="seccion portada activa" id="sec0">
        <div class="portada-contenido">
            <h1>Análisis Electoral</h1>
            <h2>de Asunción, Paraguay</h2>
            <p>Comportamiento electoral desagregado 2021-2026</p>
            <p style="font-size: 0.875rem; opacity: 0.9;">Análisis independiente basado en datos públicos del TSJE</p>
            
            <div class="portada-stats">
                <div class="stat-box">
                    <span class="numero">431.865</span>
                    <span class="label">Habilitados 2021</span>
                </div>
                <div class="divider"></div>
                <div class="stat-box">
                    <span class="numero">441.272</span>
                    <span class="label">Habilitados 2023</span>
                </div>
                <div class="divider"></div>
                <div class="stat-box">
                    <span class="numero">453.531</span>
                    <span class="label">Proyección 2026</span>
                </div>
            </div>
        </div>
        <div class="bounce-indicator">⇩</div>
    </div>

    <!-- MUNICIPALES 2021 -->
    <div class="seccion" id="sec1">
        <div class="seccion-header">
            <h1>Elecciones Municipales 2021</h1>
            <div class="divider-line"></div>
        </div>

        <div class="grid-4">
            <div class="card">
                <div class="numero" style="color: #3b82f6;">431.865</div>
                <div class="label">Total Habilitados</div>
            </div>
            <div class="card red">
                <div class="numero" style="color: #ef4444;">49.683</div>
                <div class="label">No Votaron (11,5%)</div>
            </div>
            <div class="card green">
                <div class="numero" style="color: #10b981;">176.191</div>
                <div class="label">Votaron (40,8%)</div>
            </div>
            <div class="card purple">
                <div class="numero" style="color: #8b5cf6;">29.904</div>
                <div class="label">Primera Vez (6,9%)</div>
            </div>
        </div>

        <div class="grid-2">
            <div class="chart-container">
                <h3>Participación por Rango Etario</h3>
                <canvas id="chart2021a"></canvas>
            </div>
            <div class="chart-container">
                <h3>Distribución de Primeros Votantes</h3>
                <canvas id="chart2021b"></canvas>
            </div>
        </div>

        <div class="callout blue">
            <strong>Hallazgo principal:</strong>
            El 73% de los primeros votantes en 2021 pertenecía al rango 18-24 años. La abstención fue más pronunciada en jóvenes (16,7%) respecto al promedio general (11,5%).
        </div>
    </div>

    <!-- GENERALES 2023 -->
    <div class="seccion" id="sec2">
        <div class="seccion-header">
            <h1>Elecciones Generales 2023</h1>
            <div class="divider-line"></div>
        </div>

        <div class="grid-4">
            <div class="card">
                <div class="numero" style="color: #3b82f6;">441.272</div>
                <div class="label">Total Habilitados</div>
            </div>
            <div class="card red">
                <div class="numero" style="color: #ef4444;">50.071</div>
                <div class="label">Nunca Votaron (11,3%)</div>
            </div>
            <div class="card orange">
                <div class="numero" style="color: #f59e0b;">186.347</div>
                <div class="label">Hasta 3 Ejercicios (42,2%)</div>
            </div>
            <div class="card purple">
                <div class="numero" style="color: #8b5cf6;">425</div>
                <div class="label">Primeros Votantes 2023 (0,1%)</div>
            </div>
        </div>

        <div class="grid-2">
            <div class="chart-container">
                <h3>Nunca Votantes por Rango Etario</h3>
                <canvas id="chart2023a"></canvas>
            </div>
            <div class="chart-container">
                <h3>Distribución Etaria del Padrón</h3>
                <canvas id="chart2023b"></canvas>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1rem; max-width: 80rem; margin: 0 auto;">
            <div class="callout red">
                <strong>Abstención juvenil:</strong>
                El rango 18-24 concentra 15.974 nunca votantes, equivalente al 31,9% del total. Más de una tercera parte de este grupo no participa en elecciones.
            </div>
            <div class="callout orange">
                <strong>Experiencia electoral limitada:</strong>
                42,2% del padrón tiene tres ejercicios de voto o menos, indicando una población electoral relativamente nueva en procesos electorales.
            </div>
        </div>
    </div>

    <!-- PROYECCION 2026 -->
    <div class="seccion" id="sec3">
        <div class="seccion-header">
            <h1>Proyección Padronal 2026</h1>
            <div class="divider-line"></div>
        </div>

        <div class="grid-4" style="grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));">
            <div class="card green">
                <div class="numero" style="color: #10b981;">453.531</div>
                <div class="label">Padrón Proyectado</div>
            </div>
            <div class="card">
                <div class="numero" style="color: #3b82f6;">7.496</div>
                <div class="label">Nuevos Inscritos</div>
            </div>
            <div class="card purple">
                <div class="numero" style="color: #8b5cf6;">+2,8%</div>
                <div class="label">Crecimiento 2023-2026</div>
            </div>
        </div>

        <div class="grid-2">
            <div class="chart-container">
                <h3>Nuevos Inscritos por Rango Etario</h3>
                <canvas id="chart2026a"></canvas>
            </div>
            <div class="chart-container">
                <h3>Composición de Nuevos Inscritos</h3>
                <canvas id="chart2026b"></canvas>
            </div>
        </div>

        <div class="callout green">
            <strong>Padrón en renovación:</strong>
            El 89,9% de nuevos inscritos pertenecen al rango 18-24 años, reflejando el flujo natural de ciudadanos alcanzando la mayoría de edad. La proyección 2026 mantiene un patrón de renovación esperado según dinámica demográfica.
        </div>
    </div>

    <!-- ANALISIS COMPARATIVO -->
    <di