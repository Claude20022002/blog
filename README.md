import React, { useEffect, useState } from "react";
import axios from "axios";
import Card from "./components/cards/Card";
import "./App.css";
import Header from "./components/header/Header";
import { Box, Stack } from "@mui/material";
const App = () => {
const [data, setData] = useState([]);
const [page, setPage] = useState(1);
const [totalPages, setTotalPages] = useState(1);
const [background, setBackground] = useState(
"/fotor-ai-20250213172456.jpg"
);
const [activeAnime, setActiveAnime] = useState(null);

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await axios.get(
                    `https://api.jikan.moe/v4/anime?page=${page}`
                );

                setData(response.data.data);
                setTotalPages(response.data.pagination.last_visible_page);
            } catch (error) {
                console.error("Error fetching data:", error);
            }
        };

        fetchData();
    }, [page]);

    return (
        <Box className="app">
            <Stack
                component="section"
                sx={{
                    backgroundImage: `url(${background})`,
                    backgroundSize: "cover",
                    backgroundPosition: "center",
                    backgroundRepeat: "no-repeat",
                }}
            >
                <h1>Liste des Animes</h1>
                <Stack>
                    <div className="wrapper">
                        {data.map((anime) => (
                            <Card
                                key={anime.mal_id}
                                anime={anime}
                                activeAnime={activeAnime}
                                setActiveAnime={setActiveAnime}
                                setBackground={setBackground}
                            />
                        ))}
                    </div>
                </Stack>

                <div>
                    <button
                        onClick={() => setPage(page - 1)}
                        disabled={page === 1}
                    >
                        Précédent
                    </button>

                    <span>
                        {" "}
                        Page {page} / {totalPages}{" "}
                    </span>

                    <button
                        onClick={() => setPage(page + 1)}
                        disabled={page >= totalPages}
                    >
                        Suivant
                    </button>
                </div>
            </Stack>
        </Box>
    );

};

export default App;
