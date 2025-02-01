import { useState, useEffect } from 'react';
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Progress } from "@/components/ui/progress";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";
import { Users, Briefcase, Gauge, MessageSquare, Video, Mail, Phone, Bookmark } from "lucide-react";
import { motion } from "framer-motion";

const testData = {
    companies: [
        { id: 1, name: "Future AI Solutions", industry: "Automotive", looking_for: "AI-Entwickler", required_skills: ["Reinforcement Learning", "AI Security"], location: "Berlin, Deutschland", budget: "50.000-100.000€" },
        { id: 2, name: "NeuralTech Innovations", industry: "Healthcare", looking_for: "AI-Berater", required_skills: ["Generative AI", "Computer Vision"], location: "Zürich, Schweiz", budget: "100.000-200.000€" }
    ],
    consultants: [
        { id: 1, name: "Dr. Anna Meier", role: "AI-Berater", expertise: ["NLP", "Generative AI"], location: "München, Deutschland", budget: "ab 75.000€" },
        { id: 2, name: "Lukas Weber", role: "AI-Entwickler", expertise: ["Machine Learning", "AI Security"], location: "Hamburg, Deutschland", budget: "ab 90.000€" }
    ]
};

export default function BusinessMatchApp() {
    const [successRate, setSuccessRate] = useState(75);
    const [showHologram, setShowHologram] = useState(false);
    const [selectedRole, setSelectedRole] = useState("Unternehmen");
    const [maxMatches, setMaxMatches] = useState(3);
    const [showPayment, setShowPayment] = useState(false);
    const [paymentConfirmed, setPaymentConfirmed] = useState(false);
    const [savedMatches, setSavedMatches] = useState([]);
    const [searchCriteria, setSearchCriteria] = useState({ skills: "", experience: "", description: "", location: "", budget: "", industry: "" });

    useEffect(() => {
        const timer = setTimeout(() => setShowHologram(true), 1000);
        return () => clearTimeout(timer);
    }, []);

    const displayedData = selectedRole === "Unternehmen" ? testData.consultants.slice(0, maxMatches) : testData.companies.slice(0, maxMatches);

    const saveMatch = (match) => {
        setSavedMatches([...savedMatches, match]);
    };

    return (
        <div className="p-6 space-y-6 min-h-screen bg-gradient-to-b from-gray-100 via-gray-200 to-gray-100 text-gray-900">
            <div className="text-center bg-white shadow-lg p-6 rounded-xl border border-yellow-400">
                <h2 className="text-3xl font-bold text-yellow-600">B2B: AI Business Match</h2>
                <p className="text-gray-600">Find! Match! Grow!</p>
            </div>
            
            {!showPayment ? (
                <Card className="border border-yellow-400 bg-white shadow-md">
                    <CardContent className="p-4 space-y-4 text-center">
                        <h3 className="text-lg font-semibold text-yellow-600">{selectedRole === "Unternehmen" ? "Unternehmen sucht AI-Berater" : "Berater sucht Unternehmen"}</h3>
                        <div className="space-y-4 text-left">
                            <Input placeholder="Gesuchte Skills..." value={searchCriteria.skills} onChange={(e) => setSearchCriteria({...searchCriteria, skills: e.target.value})} />
                            <Input placeholder="Erfahrung (Jahre)..." value={searchCriteria.experience} onChange={(e) => setSearchCriteria({...searchCriteria, experience: e.target.value})} />
                            <Input placeholder="Standort..." value={searchCriteria.location} onChange={(e) => setSearchCriteria({...searchCriteria, location: e.target.value})} />
                            <Input placeholder="Budget..." value={searchCriteria.budget} onChange={(e) => setSearchCriteria({...searchCriteria, budget: e.target.value})} />
                            <Input placeholder="Branche..." value={searchCriteria.industry} onChange={(e) => setSearchCriteria({...searchCriteria, industry: e.target.value})} />
                            <Textarea placeholder="Projektbeschreibung..." value={searchCriteria.description} onChange={(e) => setSearchCriteria({...searchCriteria, description: e.target.value})} />
                        </div>
                        <Button className="mt-4 bg-yellow-500 text-white px-6 py-2 rounded-lg shadow-md" onClick={() => setShowPayment(true)}>🔍 Suche starten</Button>
                    </CardContent>
                </Card>
            ) : !paymentConfirmed ? (
                <Card className="border border-yellow-400 bg-white shadow-md">
                    <CardContent className="p-4 space-y-4 text-center">
                        <h2 className="text-lg font-semibold text-yellow-600">Zugriff auf Ergebnisse</h2>
                        <p className="text-gray-700">Es wurden {maxMatches} relevante Matches gefunden! Zahlung erforderlich.</p>
                        <Button className="mt-4 bg-green-500 text-white px-6 py-2 rounded-lg shadow-md" onClick={() => setPaymentConfirmed(true)}>💳 Zahlung bestätigen</Button>
                    </CardContent>
                </Card>
            ) : (
                <Card className="border border-yellow-400 bg-white shadow-md">
                    <CardContent className="p-4 space-y-4 text-center">
                        <h2 className="text-lg font-semibold text-yellow-600">Zahlung erfolgreich!</h2>
                        <p className="text-gray-700">Ihre Matches wurden freigeschaltet.</p>
                        {displayedData.map((match) => (
                            <div key={match.id} className="p-4 border-b flex justify-between items-center">
                                <p><strong>{match.name}</strong> ({match.role || match.industry})</p>
                                <Button variant="outline" className="border-yellow-500 text-yellow-600 shadow-md flex items-center" onClick={() => saveMatch(match)}><Bookmark className="w-4 h-4 mr-2" /> Speichern</Button>
                            </div>
                        ))}
                    </CardContent>
                </Card>
            )}
        </div>
    );
}
