const S = window.PORTFOLIO_DATA;

const SECS = [
    ["home", "Home"],
    ["about", "About Me"],
    ["quiz", "Quiz"],
    ["longquiz", "Long Quiz"],
    ["exam", "Examination"],
    ["activity", "Activity"],
    ["project", "Project"]
];

const MAX = 15e6;

let first = true;
let cur = "home";
let term = "mid";
let cat = "All";
let owner = false;
let artifact = null;
let msg = "";
let view = null;
let viewUrl = "";
let armed = null;


// ===============================
// HELPERS
// ===============================

const esc = s =>
    String(s || "").replace(
        /[&<>"']/g,
        c => ({
            "&": "&amp;",
            "<": "&lt;",
            ">": "&gt;",
            '"': "&quot;",
            "'": "&#39;"
        }[c])
    );


const key = () =>
    cur === "exam"
        ? "exam-" + term
        : cur;


const items = () =>
    S.items.filter(i => i.sec === key());


const size = n => {

    if (!n || n <= 0) {
        return "Website file";
    }

    if (n > 1e6) {
        return (n / 1e6).toFixed(1) + " MB";
    }

    return Math.max(1, Math.round(n / 1e3)) + " KB";
};


const ext = n =>
    (n.split(".").pop() || "FILE")
        .slice(0, 4)
        .toUpperCase();


function toBlob(data) {

    const [h, b] = data.split(",");

    const m =
        (h.match(/:(.*?);/) || [, "application/octet-stream"])[1];

    const bin = atob(b);

    const u = new Uint8Array(bin.length);

    for (let i = 0; i < bin.length; i++) {
        u[i] = bin.charCodeAt(i);
    }

    return new Blob([u], {
        type: m
    });
}


// ===============================
// GET FILE URL
// ===============================

function getFileUrl(item) {

    // Permanent website file
    if (item.path) {
        return item.path;
    }

    // Uploaded Base64 file
    if (item.data) {

        if (
            item.type.startsWith("video/") ||
            item.type === "application/pdf"
        ) {
            return URL.createObjectURL(
                toBlob(item.data)
            );
        }

        return item.data;
    }

    return "";
}


// ===============================
// RENDER
// ===============================

function render() {

    const list = items();

    const cats = [
        "All",
        ...new Set(
            list.map(i => i.cat)
        )
    ];

    if (!cats.includes(cat)) {
        cat = "All";
    }

    const shown = list.filter(
        i =>
            cat === "All" ||
            i.cat === cat
    );

    const section =
        SECS.find(
            s => s[0] === cur
        );

    const label =
        section
            ? section[1]
            : "Portfolio";

    const examLabel =
        cur === "exam"
            ? term === "mid"
                ? " · Midterms"
                : " · Finals"
            : "";

    const allCats = [
        ...new Set(
            S.items.map(i => i.cat)
        )
    ];


    document.getElementById("app").innerHTML = `

    <header class="hero ${cur === "home" ? "" : "sm"} ${first ? "intro" : ""}">

        <div class="grid"></div>

        <i class="orb o1"></i>
        <i class="orb o2"></i>
        <i class="orb o3"></i>

        <div class="hero-in">

            ${ring()}

            <h1>
                ${
                    [...S.name]
                    .map(
                        (c, i) =>
                            `<span style="animation-delay:${i * 45}ms">
                                ${c === " " ? "&nbsp;" : esc(c)}
                            </span>`
                    )
                    .join("")
                }
            </h1>

            <p>
                ${esc(S.tag)}
            </p>

        </div>

    </header>


    <nav aria-label="Sections">

        ${SECS.map(
            s => `
                <button
                    class="pill ${cur === s[0] ? "on" : ""}"
                    data-a="sec"
                    data-v="${s[0]}">
                    ${s[1]}
                </button>
            `
        ).join("")}

    </nav>


    <main>

        ${
            cur === "home"
                ? homeView()
                : cur === "about"
                    ? aboutView()
                    : sectionView(
                        label + examLabel,
                        cats,
                        shown,
                        allCats
                    )
        }

    </main>


    <footer>

        ${esc(S.name)} · Portfolio

    </footer>


    ${view ? modal(view) : ""}

    `;

    wire();

    first = false;
}


// ===============================
// SECTION VIEW
// ===============================

function sectionView(
    label,
    cats,
    shown,
    allCats
) {

    return `

        ${
            owner
                ? `
                    <section class="owner">

                        <strong>
                            Upload to this section
                        </strong>

                        <div class="row">

                            <label for="cat">
                                Category
                            </label>

                            <input
                                type="text"
                                id="cat"
                                list="cl"
                                placeholder="e.g. Math, Programming"
                                value="${esc(
                                    cat === "All"
                                        ? ""
                                        : cat
                                )}"
                            >

                            <datalist id="cl">

                                ${allCats.map(
                                    c =>
                                        `<option value="${esc(c)}">`
                                ).join("")}

                            </datalist>


                            <label for="ti">
                                Title
                            </label>

                            <input
                                type="text"
                                id="ti"
                                placeholder="Optional (uses file name)"
                            >

                        </div>


                        <div
                            class="drop"
                            id="drop">

                            Drop files here or

                            <input
                                type="file"
                                id="f"
                                multiple
                                accept="image/*,video/mp4,video/webm,.pdf,.docx,.pptx,.xlsx,.csv,.txt,.md,.json,.zip,.html,.svg,.epub"
                            >

                        </div>


                        <div
                            class="status"
                            role="status">

                            ${esc(msg)}

                        </div>

                    </section>
                `
                : ""
        }


        <div class="sec-head">

            <h2>
                ${esc(label)}
            </h2>


            ${
                cur === "exam"
                    ? `

                        <div>

                            <button
                                class="pill ${
                                    term === "mid"
                                        ? "on"
                                        : ""
                                }"
                                data-a="term"
                                data-v="mid">

                                Midterms

                            </button>


                            <button
                                class="pill ${
                                    term === "fin"
                                        ? "on"
                                        : ""
                                }"
                                data-a="term"
                                data-v="fin">

                                Finals

                            </button>

                        </div>

                    `
                    : ""
            }

        </div>


        ${
            cats.length > 1
                ? `

                    <div class="chips">

                        ${cats.map(
                            c => `
                                <button
                                    class="pill chip ${
                                        cat === c
                                            ? "on"
                                            : ""
                                    }"
                                    data-a="cat"
                                    data-v="${esc(c)}">

                                    ${esc(c)}

                                </button>
                            `
                        ).join("")}

                    </div>

                `
                : ""
        }


        ${
            shown.length

                ? `

                    <div class="cards">

                        ${shown.map(
                            (i, n) =>
                                fileCard(i, n)
                        ).join("")}

                    </div>

                `

                : `

                    <div class="empty">

                        ${
                            owner
                                ? "Nothing here yet. Add a category and upload your first file above."
                                : "Nothing has been added to this section yet."
                        }

                    </div>

                `
        }

    `;
}


// ===============================
// FILE CARD
// ===============================

function fileCard(i, n) {

    let thumbnail = ext(i.name);

    if (
        i.type &&
        i.type.startsWith("image/") &&
        !i.type.includes("svg")
    ) {

        thumbnail = `
            <img
                alt="${esc(i.title)}"
                src="${esc(getFileUrl(i))}"
            >
        `;

    }

    return `

        <article
            class="card"
            style="animation-delay:${n * 60}ms">

            <div class="thumb">

                ${thumbnail}

            </div>


            <div class="body">

                <span class="tag">

                    ${esc(i.cat)}

                </span>


                <h3>

                    ${esc(i.title)}

                </h3>


                <div class="meta">

                    ${esc(i.name)}
                    ·
                    ${size(i.size)}

                </div>


                <div class="acts">

                    <button
                        class="btn"
                        data-a="view"
                        data-v="${i.id}">

                        View

                    </button>


                    <button
                        class="btn ghost"
                        data-a="dl"
                        data-v="${i.id}">

                        Download

                    </button>


                    ${
                        owner
                            ? `
                                <button
                                    class="btn del"
                                    data-a="del"
                                    data-v="${i.id}">

                                    ${
                                        armed === i.id
                                            ? "Sure?"
                                            : "Delete"
                                    }

                                </button>
                            `
                            : ""
                    }

                </div>

            </div>

        </article>

    `;
}


// ===============================
// PROFILE
// ===============================

function ring() {

    return `

        <div
            class="ring ${cur === "home" ? "" : "sm"}">

            ${
                S.photo
                    ? `
                        <img
                            alt="Profile photo of ${esc(S.name)}"
                            src="${S.photo}"
                        >
                    `
                    : `
                        <span class="ini">

                            ${esc(
                                S.name[0] || "?"
                            )}

                        </span>
                    `
            }

        </div>

    `;
}


function profPanel() {

    return `

        <section class="owner">

            <strong>
                Owner mode — only you see this
            </strong>


            <div class="row">

                <input
                    type="text"
                    id="pn"
                    value="${esc(S.name)}"
                    aria-label="Your name"
                >


                <input
                    type="text"
                    id="pt"
                    value="${esc(S.tag)}"
                    aria-label="Tagline"
                >

            </div>


            <textarea
                id="pa"
                aria-label="About me">

                ${esc(S.about || "")}

            </textarea>


            <div class="row">

                <label for="ph">
                    Profile photo
                </label>

                <input
                    type="file"
                    id="ph"
                    accept="image/*"
                >


                <button
                    class="btn"
                    style="flex:0 0 auto"
                    data-a="prof">

                    Save profile

                </button>

            </div>


            <div
                class="status"
                role="status">

                ${esc(msg)}

            </div>

        </section>

    `;
}


// ===============================
// HOME
// ===============================

function homeView() {

    return `

        ${
            owner
                ? profPanel()
                : ""
        }


        <div class="sec-head">

            <h2>
                Explore my work
            </h2>

        </div>


        <div class="cards">

            ${SECS.slice(2).map(
                (s, n) => {

                    const c =
                        S.items.filter(
                            i =>
                                i.sec === s[0] ||
                                i.sec.startsWith(
                                    s[0] + "-"
                                )
                        ).length;

                    return `

                        <article
                            class="card"
                            style="animation-delay:${n * 60}ms">

                            <div class="body">

                                <h3>
                                    ${s[1]}
                                </h3>


                                <div class="meta">

                                    ${c}
                                    file${c === 1 ? "" : "s"}

                                </div>


                                <div class="acts">

                                    <button
                                        class="btn"
                                        data-a="sec"
                                        data-v="${s[0]}">

                                        Open

                                    </button>

                                </div>

                            </div>

                        </article>

                    `;

                }
            ).join("")}

        </div>

    `;
}


// ===============================
// ABOUT
// ===============================

function aboutView() {

    return `

        ${
            owner
                ? profPanel()
                : ""
        }


        <div class="sec-head">

            <h2>
                About me
            </h2>

        </div>


        <div class="about">

            ${esc(
                S.about || ""
            ).replace(/\n/g, "<br>")}

        </div>

    `;
}


// ===============================
// PROFILE PHOTO
// ===============================

function setPhoto(file) {

    if (!file) return;

    const fr =
        new FileReader();

    fr.onload = () => {

        const im =
            new Image();

        im.onload = () => {

            const s =
                Math.min(
                    im.width,
                    im.height
                );

            const c =
                document.createElement(
                    "canvas"
                );

            c.width =
                c.height =
                480;

            c.getContext("2d")
                .drawImage(
                    im,
                    (im.width - s) / 2,
                    (im.height - s) / 4,
                    s,
                    s,
                    0,
                    0,
                    480,
                    480
                );

            S.photo =
                c.toDataURL(
                    "image/jpeg",
                    .85
                );

            render();

            save();

        };

        im.src =
            fr.result;

    };

    fr.readAsDataURL(file);
}


// ===============================
// MODAL
// ===============================

function modal(i) {

    const url =
        getFileUrl(i);

    let body = `

        <p class="meta">

            Preview isn't available for this file type.
            Use Download to open it.

        </p>

    `;


    if (
        i.type &&
        i.type.startsWith("image/")
    ) {

        body = `

            <img
                alt="${esc(i.title)}"
                src="${esc(url)}"
            >

        `;

    }


    else if (
        i.type &&
        i.type.startsWith("video/")
    ) {

        body = `

            <video
                controls
                src="${esc(url)}">
            </video>

        `;

    }


    else if (
        i.type === "application/pdf"
    ) {

        body = `

            <iframe
                title="${esc(i.title)}"
                src="${esc(url)}">
            </iframe>

        `;

    }


    return `

        <div
            class="modal"
            data-a="close">

            <div
                class="mbox"
                role="dialog"
                aria-label="${esc(i.title)}">

                <h3>
                    ${esc(i.title)}
                </h3>


                ${body}


                <div class="acts">

                    <button
                        class="btn ghost"
                        data-a="dl"
                        data-v="${i.id}">

                        Download

                    </button>


                    <button
                        class="btn"
                        data-a="close">

                        Close

                    </button>

                </div>

            </div>

        </div>

    `;
}


// ===============================
// UPLOAD WIRING
// ===============================

function wire() {

    const f =
        document.getElementById("f");

    const d =
        document.getElementById("drop");

    const ph =
        document.getElementById("ph");


    if (ph) {

        ph.onchange = () =>
            setPhoto(ph.files[0]);

    }


    if (f) {

        f.onchange = () =>
            add([...f.files]);

    }


    if (d) {

        d.ondragover = e => {

            e.preventDefault();

            d.classList.add("hot");

        };


        d.ondragleave = () => {

            d.classList.remove("hot");

        };


        d.ondrop = e => {

            e.preventDefault();

            d.classList.remove("hot");

            add([
                ...e.dataTransfer.files
            ]);

        };

    }

}


// ===============================
// BUTTON ACTIONS
// ===============================

document.addEventListener(
    "click",
    async e => {

        const t =
            e.target.closest(
                "[data-a]"
            );

        if (!t) return;


        if (
            t.classList.contains("modal") &&
            e.target !== t
        ) {
            return;
        }


        const a =
            t.dataset.a;

        const v =
            t.dataset.v;


        if (a === "sec") {

            cur = v;

            cat = "All";

        }


        else if (a === "term") {

            term = v;

            cat = "All";

        }


        else if (a === "cat") {

            cat = v;

        }


        else if (a === "view") {

            view =
                S.items.find(
                    i => i.id === v
                );

            viewUrl =
                getFileUrl(view);

        }


        else if (a === "close") {

            view = null;

            viewUrl = "";

        }


        else if (a === "dl") {

            return download(
                S.items.find(
                    i => i.id === v
                )
            );

        }


        else if (a === "del") {

            if (armed !== v) {

                armed = v;

            }

            else {

                S.items =
                    S.items.filter(
                        i => i.id !== v
                    );

                armed = null;

                render();

                return save();

            }

        }


        else if (a === "prof") {

            S.name =
                document
                    .getElementById("pn")
                    .value
                    .trim()
                    || "Your Name";


            S.tag =
                document
                    .getElementById("pt")
                    .value
                    .trim();


            S.about =
                document
                    .getElementById("pa")
                    .value;


            render();

            return save();

        }


        render();

    }
);


// ===============================
// DOWNLOAD
// ===============================

function download(i) {

    if (!i) return;


    // Permanent website file
    if (i.path) {

        const a =
            document.createElement("a");

        a.href =
            i.path;

        a.download =
            i.name;

        a.target =
            "_blank";

        document.body.appendChild(a);

        a.click();

        a.remove();

        return;

    }


    // Uploaded Base64 file
    if (i.data) {

        const url =
            URL.createObjectURL(
                toBlob(i.data)
            );

        const a =
            document.createElement("a");

        a.href =
            url;

        a.download =
            i.name;

        document.body.appendChild(a);

        a.click();

        a.remove();


        setTimeout(
            () =>
                URL.revokeObjectURL(url),
            1000
        );

    }

}


// ===============================
// UPLOAD
// ===============================

async function add(files) {

    const catInput =
        document.getElementById(
            "cat"
        );

    const titleInput =
        document.getElementById(
            "ti"
        );


    const c =
        (
            catInput
                ? catInput.value.trim()
                : ""
        ) || "General";


    const title =
        titleInput
            ? titleInput.value.trim()
            : "";


    let total =
        S.items.reduce(
            (s, i) =>
                s +
                (i.data
                    ? i.data.length
                    : 0),
            0
        );


    for (const file of files) {

        const data =
            await new Promise(
                r => {

                    const fr =
                        new FileReader();

                    fr.onload = () =>
                        r(fr.result);

                    fr.readAsDataURL(
                        file
                    );

                }
            );


        if (
            total +
            data.length >
            MAX
        ) {

            msg =
                `Skipped ${file.name}: the site is full (about 15 MB total). Delete something first.`;

            continue;

        }


        total +=
            data.length;


        S.items.unshift({

            id:
                "i" +
                Date.now() +
                Math.random()
                    .toString(36)
                    .slice(2, 6),

            sec:
                key(),

            cat:
                c,

            title:
                title ||
                file.name.replace(
                    /\.[^.]+$/,
                    ""
                ),

            name:
                file.name,

            type:
                file.type ||
                "application/octet-stream",

            size:
                file.size,

            data

        });

    }


    cat = c;

    render();

    save();

}


// ===============================
// SAVE
// ===============================

async function save() {

    /*
       Static GitHub Pages files are already permanent.

       Uploaded files can still be handled by
       your existing owner/publishing system
       if an artifact publisher is available.
    */

    if (!artifact) {

        msg =
            "Changes are currently local to this browser.";

        return;

    }


    msg =
        "Saving…";

    render();


    const c =
        document
            .documentElement
            .cloneNode(true);


    c.querySelector("#app")
        .innerHTML = "";


    const dataElement =
        c.querySelector("#data");


    if (dataElement) {

        dataElement.textContent =
            JSON.stringify(S)
                .replace(
                    /</g,
                    "\\u003c"
                );

    }


    try {

        await artifact.publish(
            "<!doctype html>\n" +
            c.outerHTML
        );

        msg =
            "Saved successfully.";

        render();

    }

    catch (err) {

        msg =
            "Could not save (" +
            (
                err.code ||
                err.message
            ) +
            ").";

        render();

    }

}


// ===============================
// START
// ===============================

render();
