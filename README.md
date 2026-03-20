## 🚀 Cómo ejecutar el proyecto

1. Instalar dependencias:
```bash
npm install
```

2. Crear archivo .env:
```bash
echo DATABASE_URL="file:./dev.db" > .env
```

3. Generar cliente de prisma:
```bash
npx prisma generate
```

4. Iniciar el servidor:
```bash
npm run dev
```

## 🔧 Solución de Incidentes

Durante la revisión del proyecto se detectaron comentarios en el código que sugerían posibles causas de los problemas.  
Aun así, se realizó un análisis completo y se implementaron soluciones asegurando un comportamiento correcto.

---

### 1. Botón "Resolver" no funciona en celular

**Problema:**  
En dispositivos móviles, el botón quedaba oculto debido a un footer fijo que cubría el contenido.

**Solución:**  
Se agregó un padding inferior al contenedor principal para evitar que el contenido quede oculto y permitir la interacción con el botón.

---

### 2. No hay actualización en tiempo real de los tickets

**Problema:**  
Se estaba mutando directamente el estado de React, por lo que la UI no detectaba cambios y no se actualizaba correctamente.

**Solución:**  
Se corrigió la actualización del estado creando un nuevo arreglo en lugar de modificar el existente, permitiendo que React re-renderice la vista.

---

### 3. Error al resolver tickets urgentes

**Problema:**  
El endpoint quedaba bloqueado debido a una promesa que no se resolvía correctamente.

**Solución:**  
Se corrigió la implementación asegurando que la promesa siempre se resuelva y no bloquee el flujo del endpoint.

**Mejora:** Se podría evaluar desacoplar el envío de notificaciones para evitar que afecte la respuesta del endpoint.

---

### 4. Fuga de datos entre empresas

**Problema:**  
El endpoint obtenía todos los tickets sin filtrar por empresa, permitiendo que un usuario acceda a información de otras compañías.

**Solución:**  
Se agregó un filtro por `companyId` en la consulta para asegurar que cada usuario solo acceda a los datos de su empresa.

```ts
const tickets = await prisma.ticket.findMany({
  where: { companyId: user.companyId },
  orderBy: { createdAt: 'desc' },
})
```

**Mejora:** Se recomienda centralizar el filtro por `companyId` en una capa de acceso a datos o mediante funciones helper que lo apliquen automáticamente en todas las consultas, evitando depender de su inclusión manual.  

En escenarios más avanzados, se puede implementar Row-Level Security (RLS) a nivel de base de datos para garantizar el aislamiento de datos de forma segura.