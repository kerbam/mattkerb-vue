<script setup>
import { onMounted } from "vue";

onMounted(() => {
    class FancyButton extends HTMLElement {
        connectedCallback() {
            if (this.shadowRoot) return;
            const shadow = this.attachShadow({ mode: "open" });
            const label = this.getAttribute("label") || "Button";
            const variant = this.getAttribute("variant") || "primary";
            shadow.innerHTML = `
        <style>
          button { font-family: inherit; padding: 8px 18px; border-radius: 6px; border: none; cursor: pointer; font-size: 0.875rem; }
          .primary { background: #646cff; color: white; }
          .ghost { background: transparent; color: #646cff; border: 1px solid #646cff !important; }
        </style>
        <button class="${variant}">${label}</button>
      `;
        }
    }

    class NavShell extends HTMLElement {
        connectedCallback() {
            if (this.shadowRoot) return;
            const shadow = this.attachShadow({ mode: "open" });
            shadow.innerHTML = `
        <style>
          nav { display: flex; gap: 10px; padding: 12px; border: 1px solid #ccc; border-radius: 8px; }
        </style>
        <nav></nav>
      `;
            const nav = shadow.querySelector("nav");
            const b1 = document.createElement("fancy-button");
            b1.setAttribute("label", "Dashboard");
            const b2 = document.createElement("fancy-button");
            b2.setAttribute("label", "Settings");
            b2.setAttribute("variant", "ghost");
            nav.append(b1, b2);
        }
    }

    class CustomInput extends HTMLElement {
        connectedCallback() {
            if (this.shadowRoot) return;
            const shadow = this.attachShadow({ mode: "open" });
            const placeholder = this.getAttribute("placeholder") || "";
            const label = this.getAttribute("label") || "";
            shadow.innerHTML = `
        <style>
          .wrap { display: flex; flex-direction: column; gap: 4px; }
          label { font-size: 0.8rem; color: #555; }
          input { padding: 8px 10px; border: 1px solid #ccc; border-radius: 6px; font-size: 0.875rem; font-family: inherit; outline: none; }
          input:focus { border-color: #646cff; }
        </style>
        <div class="wrap">
          <label>${label}</label>
          <input class="real-input" type="text" placeholder="${placeholder}" />
        </div>
      `;
        }
    }

    class CustomToggle extends HTMLElement {
        connectedCallback() {
            if (this.shadowRoot) return;
            const shadow = this.attachShadow({ mode: "open" });
            const label = this.getAttribute("label") || "";
            let on = this.hasAttribute("checked");
            shadow.innerHTML = `
        <style>
          .row { display: flex; align-items: center; gap: 10px; cursor: pointer; }
          .track { width: 40px; height: 22px; background: ${on ? "#646cff" : "#ccc"}; border-radius: 11px; position: relative; transition: background 0.2s; }
          .thumb { position: absolute; top: 3px; left: ${on ? "19px" : "3px"}; width: 16px; height: 16px; background: white; border-radius: 50%; transition: left 0.2s; }
          span { font-size: 0.875rem; }
        </style>
        <div class="row">
          <div class="track" id="track"><div class="thumb" id="thumb"></div></div>
          <span>${label}</span>
        </div>
      `;
            shadow.querySelector(".row").addEventListener("click", () => {
                on = !on;
                shadow.querySelector("#track").style.background = on
                    ? "#646cff"
                    : "#ccc";
                shadow.querySelector("#thumb").style.left = on ? "19px" : "3px";
            });
        }
    }

    class InfoCard extends HTMLElement {
        connectedCallback() {
            if (this.shadowRoot) return;
            const shadow = this.attachShadow({ mode: "open" });
            shadow.innerHTML = `
        <style>
          .card { border: 1px solid #ccc; border-radius: 8px; padding: 16px; display: flex; flex-direction: column; gap: 10px; }
        </style>
        <div class="card">
          <slot name="title"></slot>
          <slot name="action"></slot>
        </div>
      `;
        }
    }

    // Closed shadow roots — .shadowRoot returns null from outside
    class ClosedButton extends HTMLElement {
        connectedCallback() {
            const shadow = this.attachShadow({ mode: "closed" });
            const label = this.getAttribute("label") || "Closed Button";
            shadow.innerHTML = `
        <style>
          button { font-family: inherit; padding: 8px 18px; border-radius: 6px; border: none; cursor: pointer; font-size: 0.875rem; background: #e06c75; color: white; }
        </style>
        <button class="closed-btn">${label}</button>
      `;
        }
    }

    class ClosedForm extends HTMLElement {
        connectedCallback() {
            const shadow = this.attachShadow({ mode: "closed" });
            shadow.innerHTML = `
        <style>
          .wrap { display: flex; flex-direction: column; gap: 10px; padding: 16px; border: 1px solid #ccc; border-radius: 8px; }
          label { font-size: 0.8rem; color: #555; }
          input { padding: 8px 10px; border: 1px solid #ccc; border-radius: 6px; font-size: 0.875rem; font-family: inherit; outline: none; }
          input:focus { border-color: #e06c75; }
          button { font-family: inherit; background: #e06c75; color: white; border: none; padding: 8px 18px; border-radius: 6px; cursor: pointer; font-size: 0.875rem; align-self: flex-start; }
        </style>
        <div class="wrap">
          <label>Email</label>
          <input class="closed-input" type="email" placeholder="you@example.com" />
          <button class="closed-submit">Subscribe</button>
        </div>
      `;
        }
    }

    const components = {
        "fancy-button": FancyButton,
        "nav-shell": NavShell,
        "custom-input": CustomInput,
        "custom-toggle": CustomToggle,
        "info-card": InfoCard,
        "closed-button": ClosedButton,
        "closed-form": ClosedForm,
    };

    Object.entries(components).forEach(([name, cls]) => {
        if (!customElements.get(name)) customElements.define(name, cls);
    });

    // Dynamic shadow root on a plain div
    const host = document.getElementById("dynamic-host");
    if (host && !host.shadowRoot) {
        const shadow = host.attachShadow({ mode: "open" });
        shadow.innerHTML = `
      <style>
        button { font-family: inherit; background: #42b883; color: white; border: none; padding: 8px 18px; border-radius: 6px; cursor: pointer; font-size: 0.875rem; }
      </style>
      <button class="dynamic-btn">Dynamic Shadow Button</button>
    `;
    }
});
</script>

<template>
    <div class="testbed">
        <h2>Shadow DOM Testbed</h2>

        <section>
            <h3>Basic custom button</h3>
            <fancy-button
                label="Primary"
                data-pendo-id="basic-btn"
            ></fancy-button>
            <fancy-button label="Ghost" variant="ghost"></fancy-button>
        </section>

        <section>
            <h3>Nested shadow roots</h3>
            <nav-shell></nav-shell>
        </section>

        <section>
            <h3>Form controls</h3>
            <custom-input
                label="Username"
                placeholder="Enter username"
                data-pendo-id="username-input"
            ></custom-input>
            <custom-toggle label="Enable notifications" checked></custom-toggle>
            <custom-toggle label="Dark mode"></custom-toggle>
        </section>

        <section>
            <h3>Dynamic shadow root (plain div)</h3>
            <div id="dynamic-host"></div>
        </section>

        <section>
            <h3>Slotted content (light DOM)</h3>
            <info-card>
                <span slot="title">Slotted Title</span>
                <button slot="action" data-pendo-id="slotted-btn">
                    Slotted Action
                </button>
            </info-card>
        </section>

        <section>
            <h3>Closed shadow root — button</h3>
            <closed-button
                label="Closed Primary"
                data-pendo-id="closed-btn"
            ></closed-button>
            <closed-button label="Closed Secondary"></closed-button>
        </section>

        <section>
            <h3>Closed shadow root — form</h3>
            <closed-form data-pendo-id="closed-form"></closed-form>
        </section>
    </div>
</template>

<style scoped>
.testbed {
    max-width: 600px;
    margin: 0 auto;
    padding: 24px;
    display: flex;
    flex-direction: column;
    gap: 32px;
}

h2 {
    font-size: 1.2rem;
    font-weight: 700;
}

section {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

h3 {
    font-size: 0.8rem;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: #888;
    font-weight: 600;
}
</style>
