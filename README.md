<script>
    // 1. MASTER ADMIN PASSWORD
    const ADMIN_PASS = "ARCHER2026"; 

    // 2. INDIVIDUAL SUBSCRIBER PASSKEYS & START DATES
    const SUBSCRIBER_KEYS = {
        "CLIENT_SMITH_101": Date.now(),               // Smith's key (starts today)
        "AGENT_JONES_202": Date.parse("2026-08-01"),  // Jones's key (started Aug 1)
        "REALTOR_LEE_303": Date.now()                 // Lee's key (starts today)
    };

    function verifyAccess() {
        const inputKey = document.getElementById('auth-passcode').value.trim();
        const errorEl = document.getElementById('auth-error');

        // Check if Master Admin
        if (inputKey === ADMIN_PASS) {
            unlockStudio("Master Admin", "Unlimited Access");
            return;
        }

        // Check Individual User Key
        if (SUBSCRIBER_KEYS[inputKey]) {
            const startDate = SUBSCRIBER_KEYS[inputKey];
            const daysElapsed = (Date.now() - startDate) / (1000 * 60 * 60 * 24);

            // Active 30-Day Window Check
            if (daysElapsed <= 30) {
                const daysLeft = Math.max(0, Math.ceil(30 - daysElapsed));
                unlockStudio(inputKey, `${daysLeft} Days Left`);
                return;
            } else {
                errorEl.innerText = "Your 30-day access key has expired. Please contact Patrick to renew.";
                errorEl.classList.remove('hidden');
                return;
            }
        }

        // Invalid Key
        errorEl.innerText = "Invalid passkey. Please check your key or request access.";
        errorEl.classList.remove('hidden');
    }

    function unlockStudio(userLabel, badgeText) {
        document.getElementById('auth-lock-overlay').classList.add('hidden');
        document.getElementById('studio-workspace').classList.remove('hidden');
        document.getElementById('access-timer-badge').innerHTML = `User: <strong class="text-amber-400">${userLabel}</strong> | Status: <strong class="text-emerald-400">${badgeText}</strong>`;
    }

    function lockStudio() {
        document.getElementById('auth-lock-overlay').classList.remove('hidden');
        document.getElementById('studio-workspace').classList.add('hidden');
    }
</script>
